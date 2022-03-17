---
title: MceManualMotion
type: lib
layout: function
description: |
  Handling manual motions for a MotoLogix system.
tags:
  - jog
categories: examples
---

Whenever you need to *adjust* a position, *teach* a user frame,
*calibrate* a tool or move the robot for *maintenance* some manual
motions are required.

The MotoLogix library includes a set of jog functions
(`MLxRobotJogAxes`, `MLxRobotJogTCP` etc.) for this matter.
However, in this module we are using a different approach by using only
*regular motion commands*.
The jog motion is created by a flow of relative motion commands using
{{< link MceRelativeAxisMotions >}} and {{< link MceRelativeTcpMotions >}}.

**Advantages:**

- Support *inching* (move a fixed small distance per click).
- Independent jog operation on systems with multiple robots
  (`R1` can be jogged while `R2` is running its normal cycle).
- Allows fine tuning.
- Does not require adjusting robot parameters (e.g. in case of slow PLC task).
- No speed restriction.

**Disadvantages:**

- Requires some effort to determine the
  {{< link "Relative motion deltas" "motion deltas">}}.
  This has to be done *once for every type* of motion device
  (e.g. GP12 manipulator).

{{< note >}}
Since `MceManualMotion` is not using regular MotoLogix jog commands
the jogging mode is not used (`MLX.JoggingMode = 0`).
{{< /note >}}

**Features:**

- Jog axis (joints)
- Jog TCP (cartesian motion)
- Inching axis
- Inching TCP
<!-- - Execute PosTable entry -->

**State machines:**

- {{< link "MceManualMotionIO#nSmManualMotion" "nSmManualMotion">}} - handling
  the manual motion
- {{< link "MceManualMotionIO#nSmSettings" "nSmSettings">}} - update settings
  for cartesian motion

## Usage

Usually you only need one instance of this module -- even if you have multiple
MotoLogix systems and/or motion devices.
By using a selector on the HMI you map the manual motion module to the desired
system and motion device.

{{< note warning >}}
Switching to a different MotoLogix system is only allowed at manual motion
standstill.
{{< /note >}}

If you want to be able to perform manual motions *simultaneous* on multiple
MotoLogix systems and/or motion devices you would need to create additional
instances.

Create the (global) variable for the interface data.
This is also used for connecting buttons or an HMI.

```iecst
stManualMotion : MceManualMotionIO; // data for manual motion of MotoLogix motion devices
```

Create the instance:

```iecst
fbManualMotion : MceManualMotion;
```

Map all relevant inputs (see {{< link "MceManualMotionIO" >}})
to your HMI and/or other programs:

```iecst
// this is just a portion of the relevant signals
GVL.stManualMotion.nMlxNumber := ...;
GVL.stManualMotion.nRobotNumber := ...;
GVL.stManualMotion.nOperatingMode := ...;
GVL.stManualMotion.bSystemReady := ...;
GVL.stManualMotion.bInchingMode := ...;
GVL.stManualMotion.aJogRobotAxisNeg := ...;
GVL.stManualMotion.aJogRobotAxisPos := ...;
GVL.stManualMotion.nJogType := ...;
GVL.stManualMotion.nCoordFrame := ...;
```

Call the instance:

```iecst
// function call
fbManualMotion(
  io := GVL.stManualMotion,
  UserFrames := GVL.stUserFrames[<robot selection>],
  MotionDeltas := GVL.stMotionDeltas[<robot selection>],
  MLX := GVL.stMLX[<mlx selection>]);
```

Map all relevant outputs (see {{< link "MceManualMotionIO" >}})
to your HMI and/or other programs:

```iecst
// this is just a portion of the relevant signals
... := GVL.stManualMotion.bIdle;
... := GVL.stManualMotion.bBusy;
... := GVL.stManualMotion.bDone;
... := GVL.stManualMotion.aJogRobotAxisNegIndicator;
... := GVL.stManualMotion.aJogRobotAxisPosIndicator;
```
