---
title: MceAlarms
type: lib
layout: function
description: |
  Handling the alarms for a MotoLogix system.
tags: general
categories: examples
---

The MotoLogix data packet has a variable which tells the amount of active
alarms. To read the alarm information we need to call `MLxGetErrorDetail`
for each active alarm.
That function reads the information as strings (in the active pendant language).

The alarm information is stored in an array
(see {{< link "MceAlarmsIO#aAlarms" "MceAlarmsIO" >}}).

This function uses two states machines:

- {{< link "MceAlarmsIO#nSmReadAlarms" "nSmReadAlarms">}} - getting the alarm
  information from the controller
- {{< link "MceAlarmsIO#nSmResetAlarm" "nSmResetAlarm">}} - resetting alarms

## Usage

{{< note >}}
Each MotoLogix system needs its own `MceAlarms` instance.
{{< /note >}}

Create the (global) variables for the interface data.
These are also used for connecting buttons or an HMI.


```iecst
stAlarms : ARRAY [0..GVL.MLX_UBOUND] OF MceAlarmsIO; // data for alarm handling of a MotoLogix system
```

Create the instances:

```iecst
fbAlarms : ARRAY [0..GVL.MLX_UBOUND] OF MceAlarms;
```

Map all relevant inputs (see {{< link "MceAlarmsIO" >}})
to your HMI.

```iecst
GVL.stAlarms[0].bReset := ...;
```

Call the instances in a loop:

```iecst
// function call
FOR i := 0 TO GVL.MLX_UBOUND DO
  fbAlarms[i](
    io := GVL.stAlarms[i],
    blinkSignals := GVL.stBlinkSignals,
    MLX := GVL.stMLX[i]);
END_FOR;
```

Map all relevant outputs (see {{< link "MceAlarmsIO" >}})
to your HMI and/or to higher level state machines:

```iecst
// this is just a portion of the relevant signals
... := GVL.stAlarms[0].bResetIndicator;
... := GVL.stAlarms[0].nAlarms;
... := GVL.stAlarms[0].aAlarms;
```
