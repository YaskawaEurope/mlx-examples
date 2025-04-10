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

**Features:**

- Jog axis (joints)
- Jog TCP (cartesian motion)
- Inching axis
- Inching TCP
- Jog to point (point-to-point or linear)
- Handles the switching of `MLX.JoggingMode`.
- Activates the selected Tool and User Frame for cartesian jog motions.

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
GVL.stManualMotion.bMoveTo := ...;
GVL.stManualMotion.aTargetPosition := ...;
GVL.stManualMotion.aTargetType := ...;
```

Call the instance:

```iecst
// function call
fbManualMotion(
  io := GVL.stManualMotion,
  UserFrames := GVL.stUserFrames[<robot selection>],
  MLX := GVL.stMLX[<mlx selection>]);
```

Map all relevant outputs (see {{< link "MceManualMotionIO" >}})
to your HMI and/or other programs:

```iecst
// this is just a portion of the relevant signals
... := GVL.stManualMotion.bIdle;
... := GVL.stManualMotion.bBusy;
... := GVL.stManualMotion.bDone;
... := GVL.stManualMotion.bError;
... := GVL.stManualMotion.aJogRobotAxisNegIndicator;
... := GVL.stManualMotion.aJogRobotAxisPosIndicator;
```

Jogging to a target position does not insure that the TCP speed is limited to
250 mm/s.
This is because standard motion commands `MLxRobotMoveAxisAbsolute` and
`MLxRobotMoveLinearAbsolute` are used.

For an experience similar to the teach pendant when moving the robot to a target
position, a speed factor is applied to the `nSpeed` input.
These factors are hardcoded in the function block.

For example, for a linear motion (`nJogType = 1`) and `nSpeed = 20`, the speed
passed to the motion command is *2%* (*10%* of *20%*).

| `nJogType` value | Speed factor |
| ---------------- | ------------ |
| 0                | 3%           |
| 1                | 10%          |

{{< note warning >}}
Speed parameter does not limit the TCP speed to *250 mm/s*.
For safety reasons, make sure to use TCP speed limit function when safety fences
are opened.
{{< /note >}}

Acceleration and deceleration default values has been selected to mimic the
robot behavior when using the teach pendant.

| Parameter | Value |
| --------- | ----- |
| `nAcc`    | 40%   |
| `nDec`    | 80%   |
