---
title: MceAlarms
type: lib
layout: function
description: |
  Alarm handling for a MotoLogix system.
tags: general
categories: examples
---

For this purpose it uses two states machines:

- {{< link "MceAlarmsIO#nSmReadAlarms" "nSmReadAlarms">}} - getting the alarm
  information from the controller
- {{< link "MceAlarmsIO#nSmResetAlarm" "nSmResetAlarm">}} - resetting alarms

## Usage

{{< note >}}
Each MotoLogix system needs its own `MceAlarms` instance.
{{< /note >}}

We start by creating the (global) variables for the *interface data*.
These variables are also used for connecting buttons or an HMI.


```iecst
stAlarms : ARRAY [0..GVL.MLX_UBOUND] OF MceAlarmsIO; // data for alarm handling of a MotoLogix system
```

Now we create the *instances*:

```iecst
FB_MceAlarms : ARRAY[0..GVL.MLX_UBOUND] OF MceAlarms;
```

Map *all relevant inputs* (see {{< link "MceAlarmsIO" >}})
to your HMI.

```iecst
GVL.stAlarms[0].bReset := ...;
```

Since we defined the variables as arrays we can process the *function calls*
in a loop:

```iecst
// function call
FOR i := 0 TO GVL.MLX_UBOUND DO
  FB_Alarms[i](
    io := GVL.stAlarms[i],
    blinkSignals := GVL.stBlinkSignals,
    MLX := GVL.stMLX[i]);
END_FOR;
```

Map *all relevant outputs* (see {{< link "MceAlarmsIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
... := GVL.stAlarms[0].bResetIndicator;
... := GVL.stAlarms[0].nAlarms;
... := GVL.stAlarms[0].aAlarmData;
```
