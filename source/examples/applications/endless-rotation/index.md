---
title: Endless rotation
description: |
  Operating endless-rotation axes with MotoLogix.
tags:
 - endless rotation
categories:
  - learn
---

{{< lead />}}

Endless motion is an *option function* which can be applied to external axes
(e.g. `S1`) and in some cases also for selected manipulator axes
(e.g. `T-axis`).



MotoLogix does not have native support for axes with endless motion.
However, by sending a series of relative motion commands we can add some of the
functionality ourselves.

## Library components

{{< lib-items tag="endless rotation" type="data type">}}
{{< lib-items tag="endless rotation" type="function block">}}
{{< lib-items tag="endless rotation" type="function">}}

## Option function setting

The option function has to be set by Yaskawa.

![endless-function](endless-function.jpg "Settings for the endless function.")

| item             | value                      |
| ---------------- | -------------------------- |
| ENDLESS FUNCTION | `STANDARD`<br>`AUTO-RESET` |

- **AUTO-RESET** automatically resets the axis position to keep it in the range
  of -180 to 180 deg. This can be useful if besides endless rotation it is also
  required to do absolute positioning (e.g. move to 90 deg). Downside it that
  it does not allow relative motion ranges larger then 180 deg. This makes
  it impossible to do endless motion at higher speeds.
- **STANDARD** does not automatically reset the axis position. This allows larger
  relative motion distances which makes it suitable for higher speeds.
  But since the axis position is never reset, absolute positioning is not so
  comfortable (e.g. moving to absolute 90 deg could lead to rotating many
  revolutions).

## How it works

A small state machine handles three instances of the relative
axis motion command (`MLxRobotMoveAxisRelative`).

- As long as motion is requested (`runAxis <> 0`), the state machine starts
  looping in state `10/11/12` and initiating motion commands.
- When motion is no longer requested (`runAxis = 0`) the state machine will wait
  until all sent motions are finished and then return to the idle state.
  The time this takes depends on the combination of `speed` and `stepAngle`. 

It uses the principle where each state has a *primary motion* and sends up to
*two motions in advance*.
This way the robot controller always has the required three motions (for
smooth blending) in its motion queue.

```iecst
//start this motion
moveCmd[0].Enable := TRUE;

//queue next 2 motions
moveCmd[1].Enable := (moveCmd[0].Sts_EN AND moveCmd[0].Sts_DN) AND (QA >= 1);
moveCmd[2].Enable := (moveCmd[1].Sts_EN AND moveCmd[1].Sts_DN) AND (QA >= 2);
```

- By using the `Sts_DN` of the preceding command the commands are
  send nicely after each other.

- The `QA` variable (Queueing Amount) is used as mechanism to work towards
  a standstill.
  In such case the state machine should no longer keep sending motions in
  advance but it should still handle the already sent motions.

The QA is being recalculated at every transition between these three states.
A transition takes place when the primary motion is finished (`Sts_PC`).

```iecst
IF moveCmd[0].Sts_EN AND moveCmd[0].Sts_PC THEN
  moveCmd[0].Enable := FALSE;
  // calc QA
  IF run THEN
    QA := 2;      // no standstill foreseen
  ELSE
    QA := QA - 1; // planning towards standstill
  END_IF;

  IF (QA >= 0) THEN
    state[0] := 11;
  ELSE
    state[0] := 0;
  END_IF;
END_IF;
```

## Higher speeds

By default, the `stepAngle` is set to 120 degrees, which means that for each
full revolution of the axis three motion commands are sent.
This works well with both the `STANDARD` and `AUTO-RESET` setting (as the latter
only allows stepAngles up to `180 deg`).

If higher speeds are required, such small stepAngles are not sufficient.
The duration of sending a motion command will be longer than the motion
itself, resulting in non-smooth motion and/or capping the speed.

In this case the `stepAngle` has to be increased, e.g. to several
thousand degrees (only possible when using the `STANDARD` setting).

{{< note >}}
By increasing the `stepAngle` linear with the set `speed` you can handle
high speeds but still keep short stopping times at low speeds.
{{< /note >}}

## Availability

Please refer to the function block documentation to see which robot controllers
support this function.

## Parameters

Below overview shows the relevant parameters for this function.

```text
S2C869 = 1
S2C1589 = 1
```


## Example

A typical use case is a robot used for CNC machine tending:

- While moving inside the machine a rather sensitive collision detection
  is desired.
- While moving outside the machine very fast motions are used to achieve fast
  cycle times. These require higher detection levels.

Below pseudo code gives you an idea of the program structure for this use case.

```iecst
// write files in startup routine
10  SetCollisionDetectionPropertiesHi   // e.g. FileNumber := 0

20  SetCollisionDetectionPropertiesLo   // e.g. FileNumber := 1

// -----------------------------------------------------------------------------

// production
10  SelectCollisionDetectionHi          // select high detection levels
                                        // FileNumber := 0

20  <some motion instructions>          // these motion instructions are using
                                        // high speed and thus require higher
                                        // torque levels
                                        // (transition: Sts_PC)

30  SelectCollisionDetectionLo          // select low detection levels
                                        // FileNumber := 1

40  <some motion instructions>          // these motion instructions are leading
                                        // the robot into the CNC machine and
                                        // require low torque levels to
                                        // prevent damage
                                        // (transition: Sts_PC)
```
