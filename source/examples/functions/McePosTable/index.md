---
title: McePosTable
type: lib
layout: function
description: |
  Run trajectories on a MotoLogix system, using a dynamic program where the 
  trajectory is defined by data, not by the program code.
tags: 
  - PosTable
categories: examples
---

## Usage

{{< note >}}
Each motion device needs its own `McePosTable` instance.
There can be up to 4 motion devices on a robot controller.
{{< /note >}}

The `McePosTable` function block relies on several global constants that must
be defined.

```iecst
VAR CONSTANT
  TOOLS_UBOUND :      INT := 9;   (*Tools array; upper boundary*)
  USERFRAMES_UBOUND : USINT := 9; (*User frames; upper boundary*)
  ENTRIES_UBOUND :    INT := 19;  (*PosTable array; upper boundary*)
  NR_OF_INSTANCES :   USINT := 3; (*Number of motion command instances*)
END_VAR
```

| Constant          | Lower limit | Upper limit | Recommended value |
| ----------------- | :---------: | :---------: | :---------------: |
| TOOLS_UBOUND      |      0      |    63\*     |         9         |
| USERFRAMES_UBOUND |      0      |    62\*     |         9         |
| ENTRIES_UBOUND    |      1      |     N/A     |        19         |
| NR_OF_INSTANCES   |      2      |     20      |         3\**      |

\* These numbers are limited by the robot controller.
{{< note info >}}
Some numbering in the robot controller starts at 1 instead of 0, meaning
that there can be an offset between MotoLogix and the data seen in the robot pendant.
{{< /note >}}

\** The robot controller's path comment: \L$1r can calculate blending nicely when there
are three motion commands in its buffer. Having more commands in the buffer
(by increasing the number of motion command instances) is seldom useful
and consumes more PLC resources.

{{< note warning >}}
If you decide to increase `NR_OF_INSTANCES` you need to pay close attention on
not overfilling the MotoLogix command buffer (see `MLxxInternalData`).
{{< /note >}}

We start by creating the (global) variables for the interface data.
These variables are also used for connecting buttons or an HMI.

```iecst
stPosTable : ARRAY [0..GVL.MOTION_DEVICES_UBOUND] OF McePosTableIO; // data to handle a robot trajectory
```

{{< note >}}
Using a global constant to set the array size makes life easier.
{{< /note >}}

Now we create the instances:

```iecst
fbPosTable : ARRAY [0..GVL.MOTION_DEVICES_UBOUND] OF McePosTable;
```

Map all relevant inputs (see {{< link "McePosTableIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
GVL.stPosTable[0].nRobotNumber := ...;
GVL.stPosTable[0].bSystemReady := ...;
GVL.stPosTable[0].bRun := ...;
GVL.stPosTable[0].bStep := ...;
GVL.stPosTable[0].nMode := ...;
GVL.stPosTable[0].nNrOfCycles := ...;
```

For the function call, make sure to link it to the right MLX data structure.

```iecst
// function call
fbPosTable[0](
  io := GVL.stPosTable[0],
  MLX := GVL.stMLX[0],
  PosTable := myPosTable,
  UserFrames := GVL.stUserFrames[0],
  Tools := GVL.stTools[0]);
```

Map all relevant outputs (see {{< link "McePosTableIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
... := GVL.stPosTable[0].bIdle;
... := GVL.stPosTable[0].bBusy;
... := GVL.stPosTable[0].bWaitForStep;
... := GVL.stPosTable[0].bDone;
... := GVL.stPosTable[0].bError;
... := GVL.stPosTable[0].nErrorCode;
... := GVL.stPosTable[0].nPercentComplete;
... := GVL.stPosTable[0].nStateTime;
... := GVL.stPosTable[0].nIndex;
... := GVL.stPosTable[0].nCycleNr;
```

### System state mapping

To run a trajectory, the controller needs to be in SERVO ON state.
This state must be input to the `bSystemReady` variable
(see {{< link "McePosTableIO" >}}).

If you use the {{< link "MceStartStop" >}} function block, this status is available
as the `bSystemReady` output.
Simply link them as below.

```iecst
GVL.stPosTable[0].bSystemReady := GVL.stStartStop[0].bSystemReady;
```

### Use ActionID

*ActionID* are actions run at the end of a motion.

| ActionID | Type of action     |
| -------- | ------------------ |
| 0        | No action          |
| 1-19     | Default action |
| >19      | Custom action  |

For `ActionID = 0`, no action is performed at the end of the motion.
The trajectory is blended depending on the selected `nMode`.

The default *ActionID* are listed below.
They are handled inside `McePosTable`.

| ActionID | Action                     |
| -------- | -------------------------- |
| 1        | Wait one PLC task scan     |
| 2        | Wait a `bStep` rising edge |

Custom *ActionID* number are greater or equal to `20`.
You can create as many as you want (max `32767`).

{{< link "McePosTableIO" >}} has several I/O dedicated for Custom *ActionID*.

```iecst
// Input
GVL.stPosTable[0].bCustomActionDone := ...; // The custom ActionID process is completed

//Output
... := GVL.stPosTable[0].bActionStart;      // An ActionID has started (default or custom)
... := GVL.stPosTable[0].nActionID;         // Current ActionID number
```

The example below add 2 Custom *ActionID*: close and open gripper.

```iecst
IF GVL.stPosTable[0].bActionStart THEN
  CASE GVL.stPosTable[0].nActionID OF
    20:
      // Close gripper
      GVL.stPosTable[0].bCustomActionDone := GripperCloseTime.Q;
      
    21:
      // Open gripper
      GVL.stPosTable[0].bCustomActionDone := GripperOpenTime.Q;
      
  END_CASE;
ELSE
  GVL.stPosTable[0].bCustomActionDone := FALSE;
END_IF;
```

### McePosTable controlled by a higher-level state machine

`McePosTable` is typically controlled by a higher-level state machine
that prepares the data for the desired trajectory and commands `McePosTable` to
execute that trajectory.

Below pseudo code gives an example of a such a higher-level state machine.

```iecst
// Process to run 2 different trajectories
stPosTable[0].bRun := False; // init cyclically

CASE 
  ...

  // State 40 - generate trajectory 1
  40:
    GenerateTrajectory(trajType := 1, posTable:= myPosTable); // Write data for trajectory 1
    // Go to next state

  // State 50 - Run trajectory 1
  50:
    stPosTable[0].bRun := True; // Run McePosTable
    IF stPosTable[0].bDone THEN // McePosTable done
      // Go to next state
    END_IF;

  // State 60 - generate trajectory 2
  60:
    GenerateTrajectory(trajType := 2, posTable:= myPosTable); // Write data for trajectory 2
    // Go to next state

  // State 70 - Run trajectory 2
  70:
    stPosTable[0].bRun := True; // Run McePosTable
    IF stPosTable[0].bDone THEN // McePosTable done
      // Go to next state
    END_IF;

  ...
END_CASE;
```
