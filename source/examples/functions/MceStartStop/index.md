---
title: MceStartStop
type: lib
layout: function
description: |
  Start/stop logic for a MotoLogix system.
  This also implements a robot controller speed override.
tags: system
categories: examples
---

For these purposes it uses two states machines:

- {{< link "MceStartStopIO#nSmStartStop" "nSmStartStop">}} - handling the start/stop (resulting in changing the `MLX.systemState`)
- {{< link "MceStartStopIO#nSmSpeedOverride" "nSmSpeedOverride">}} - handling the controller speed override

## Usage

{{< note >}}
Each MotoLogix system needs its own `MceStartStop` instance.
{{< /note >}}

We start by creating the (global) variables for the *interface data*.
These variables are also used for connecting buttons or an HMI.


```iecst
stStartStop : ARRAY [0..GVL.MLX_UBOUND] OF MceStartStopIO; // data for start/stop of a MotoLogix system
```

{{< note >}}
Using a global constant to set the array size makes life easier.
{{< /note >}}

Now we create the *instances*:

```iecst
fbMceStartStop : ARRAY [0..GVL.MLX_UBOUND] OF MceStartStop;
```

Map *all relevant inputs* (see {{< link "MceStartStopIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
GVL.stStartStop[0].bStart := ...;
GVL.stStartStop[0].bStop := ...;
GVL.stStartStop[0].bHoldRestart := ;
GVL.stStartStop[0].fSpeedOverride := ...;
GVL.stStartStop[0].bExternalConditionsOk := ...;
```

Since we defined the variables as arrays we can process the *function calls*
in a loop:

```iecst
// function call
FOR i := 0 TO GVL.MLX_UBOUND DO
  fbStartStop[i](
    io := GVL.stStartStop[i],
    MLX := GVL.stMLX[i]);
END_FOR;
```

Map *all relevant outputs* (see {{< link "MceStartStopIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
... := GVL.stStartStop[0].bServoOn;
... := GVL.stStartStop[0].bIdle;
... := GVL.stStartStop[0].bHoldActive;
```
