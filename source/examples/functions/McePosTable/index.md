---
title: McePosTable
type: lib
layout: function
description: |
  Run trajectories on a MotoLogix system, using a dynamic program where the 
  trajectory is defined by data, not by the program code.
tags: 
  - PosTable
  - beta
categories: examples
---

## Global constants

The `McePosTable` function block relies on several global constants that must
be defined.

```iecst
VAR CONSTANT
  TOOLS_UBOUND :            INT := 9;   (*Tools array; upper boundary*)
  USERFRAMES_UBOUND :       USINT := 9; (*User frames; upper boundary*)
  ENTRIES_UBOUND :          INT := 19;  (*PosTable array; upper boundary*)
  NR_OF_INSTANCES :         USINT := 3; (*Number of motion command instances*)
  NR_OF_INTERLOCK_FLAGS :   USINT := 31; (*Number of interlock flags*)
END_VAR
```

| Constant              | Lower limit | Upper limit | Recommended value |
| --------------------- | :---------: | :---------: | :---------------: |
| TOOLS_UBOUND          |      0      |    63\*     |         9         |
| USERFRAMES_UBOUND     |      0      |    62\*     |         9         |
| ENTRIES_UBOUND        |      1      |     NA      |        19         |
| NR_OF_INSTANCES       |      2      |     20      |         3         |
| NR_OF_INTERLOCK_FLAGS |      0      |     NA      |        31         |

\* The number of Tool and User Frame are limited by the robot controller.
However, the number of User Frame stored in the PLC can be greater than `62`,
as long as the `UserFrameNumber` passed to a `MLxRobotSetUserFrame` function
block is lower or equal to `62`.

## Usage

{{< note >}}
Each motion device needs its own `McePosTable` instance.
There can be up to 4 motion devices on a robot controller.
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

## System state mapping

To run a trajectory, the controller needs to be in SERVO ON state.
This state must be input to the `bSystemReady` variable
(see {{< link "McePosTableIO" >}}).

If you use the {{< link "MceStartStop" >}} function block, this status is available
as the `bSystemReady` output.
Simply link them as below.

```iecst
GVL.stPosTable[0].bSystemReady := GVL.stStartStop[0].bSystemReady;
```

## Use ActionID

*ActionID* are actions run at the end of a motion.

| ActionID | Type of action |
| -------- | -------------- |
| 0        | No action      |
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
      
  END_CASE
ELSE
  GVL.stPosTable[0].bCustomActionDone := FALSE;
END_IF
```

## McePosTable controlled by a higher-level state machine

`McePosTable` is most of the time controlled by a higher-level state machine
that select the current trajectory to run and monitor the `McePosTable` status.

Below pseudo code gives an example of a such a higher-level state machine.

```iecst
// State 0 - Initialize McePosTable
00:
  stPosTable[0].nRobotNumber := 0;    // Link McePosTable to Robot 1
  stPosTable[0].bRun := False;        // Reset McePosTable
  stPosTable[0].nMode := 0;           // Continuous mode
  stPosTable[0].nNrOfCycles := 1;     // Run only 1 cycle

  IF stStartStop[0].bSystemReady THEN // Wait for the robot controller to be ready
    // Go to state 10
  END_IF;

// State 10 - Reset McePostable and generate a new trajectory
10:
  stPosTable[0].bRun := False;        // Reset McePosTable
  PosTable := GenerateTrajectory();   // Write trajectory in a McePosTableData structure

  IF stPosTable[0].bIdle THEN         // McePosTable in Idle
    // Go to state 20
  END_IF;

// State 20 - Launch trajectory
20:
  stPosTable[0].bRun := True;         // Run McePosTable

  IF stPosTable[0].bBusy THEN         // McePosTable busy
    // Go to state 30
  END_IF;

// State 30 - Wait for trajectory to complete
30:
  // Nothing to do

  IF stPosTable[0].bDone THEN         // McePosTable done
    // Go to state 10
  END_IF;
```
