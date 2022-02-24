---
title: MceActualPositions
type: lib
layout: function
description: |
  Read actual positions and speed from a MotoLogix system.
tags: general
categories: examples
---

This function reads the *TCP speed* and the various *actual positions* of
each motion device in a MotoLogix system:

- TCP position in the world frame
- TCP position in the active user frame
- axis position

{{< note >}}
Reading TCP speed requires parameter `S2C1702=1`
{{< /note >}}

This function uses a state machine:

- {{< link "MceActualPositionsIO#nSmActualPosition" "nSmActualPosition">}} -
  getting actual position information from the MotoLogix system.
It stores the position and speed information in arrays,
see {{< link "MceActualPositionsIO#aPositions" "aPositions" >}} and
{{< link "MceActualPositionsIO#aTcpSpeeds" "aTcpSpeeds" >}}.

{{< note >}}
The position readings use different mechanisms.
{{< /note >}}

Axis positions have their own channels and are updated cyclically.
TCP positions share one communication channel and have to be red one
after the other which leads to less frequent (but still fast) updates.

On a system with two robots this results in following read sequence:

1. TCP **world** (`R1`) + all axis positions
1. TCP **user** (`R1`) + all axis positions
1. TCP **world** (`R2`) + all axis positions
1. TCP **user** (`R2`) + all axis positions

{{< note >}}
The read sequence can be temporarily *locked to a specific robot/frame* for
fastest possible TCP speed and position updates, see
{{< link "MceActualPositionsIO#nLockTarget" "nLockTarget">}}.
{{< /note >}}

## Usage

{{< note >}}
Each MotoLogix system needs its own `MceActualPositions` instance.
{{< /note >}}

Create the (global) variables for the interface data.
These are also used for connecting to an HMI.

```iecst
stActualPositions : ARRAY [0..GVL.MLX_UBOUND] OF MceActualPositionsIO; // data for reading positions of a MotoLogix system
```

Map all relevant inputs (see {{< link "MceActualPositionsIO" >}})
to your HMI and/or to other programs:

```iecst
GVL.stActualPositions[0].bUseFeedbackPosition := ...;
GVL.stActualPositions[0].nLockTarget := ...;
```

Create the instances:

```iecst
fbActualPositions : ARRAY [0..GVL.MLX_UBOUND] OF MceActualPositions;
```

Call the instances in a loop:

```iecst
// function call
FOR i := 0 TO GVL.MLX_UBOUND DO
  fbActualPositions[i](
    io := GVL.stActualPositions[i],
    MLX := GVL.stMLX[i]);
END_FOR;
```

Map all relevant outputs (see {{< link "MceActualPositionsIO" >}})
to your HMI and/or to other programs:

```iecst
// this is just a portion of the relevant signals
... := GVL.stActualPositions[0].aTcpSpeeds[0];
... := GVL.stActualPositions[0].aPositions[0].aWorld;
```
