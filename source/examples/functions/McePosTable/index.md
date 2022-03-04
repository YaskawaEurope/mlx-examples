---
title: McePosTable
type: lib
layout: function
description: |
  Performed a trajectory stored in a *PosTable*.
tags: system
categories: examples
---

The *PosTable* system is used to perform a trajectory using the minimum number
of MotoLogix Function Blocks.

## Usage

{{< note >}}
Each motion device needs its own `McePosTable` instance.
There can be up to 4 motion devices on a robot controller.
{{< /note >}}

We start by creating the (global) variables for the *interface data*.
These variables are also used for connecting buttons or an HMI.

```iecst
stPosTable : ARRAY [0..GVL.MOTION_DEVICES_UBOUND] OF McePosTableIO; // data to handle a robot trajectory
```

{{< note >}}
Using a global constant to set the array size makes life easier.
{{< /note >}}

Now we create the *instances*:

```iecst
fbPosTable : ARRAY [0..GVL.MOTION_DEVICES_UBOUND] OF McePosTable;
```

Map *all relevant inputs* (see {{< link "McePosTableIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
GVL.stPosTable[0].nRobotNumber := ...;
GVL.stPosTable[0].bSystemReady := ...;
GVL.stPosTable[0].bStep := ...;
GVL.stPosTable[0].bCmdStart := ...;
GVL.stPosTable[0].bRecalcQA := ...;
GVL.stPosTable[0].nNrOfCycles := ...;
GVL.stPosTable[0].nPosTableMode := ...;
GVL.stPosTable[0].fSpeedManipulation := ...;
```

For the *function call*, make sure to link it to the right *MLX* data structure.

```iecst
// function call
fbPosTable[0](
	io := GVL.stPosTable[0],
	MLX := GVL.stMLX[0],
	PosTable := myPosTable,
	UserFrames := GVL.stUserFrames[0],
	Tools := GVL.stTools[0]);
```

Map *all relevant outputs* (see {{< link "McePosTableIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
... := GVL.stPosTable[0].bRunning;
... := GVL.stPosTable[0].bWaitForStep;
... := GVL.stPosTable[0].nPercentComplete;
... := GVL.stPosTable[0].nStateTime;
```
