---
title: MceWarnings
type: lib
layout: function
description: |
  Reading the warnings of a MotoLogix system.
tags: general
categories: examples
---

MotoLogix *warnings* are handled quite differently than MotoLogix *alarms*.
There is no variable in the data packet which tells the amount of active
warnings. Instead, we need to *poll* for warnings using `MLxGetMessageDetail`.

{{< note >}}
A full polling sequence consists of 10 polling actions.
{{< /note >}}

For this purpose it uses a state machine:

- {{< link "MceWarningsIO#nSmReadWarnings" "nSmReadWarnings">}} - getting the
  warning information from the controller

With polling it is important to find a balance between *quick readings* (of
new warning data) and *not holding up* other (more important) MotoLogix
commands:

- The polling time can be adjusted to your setup and demands
- The polling automatically slows down by *factor five* under certain conditions

## Usage

{{< note >}}
Each MotoLogix system needs its own `MceWarnings` instance.
{{< /note >}}

Create the (global) variables for the *interface data*.
These are also used for connecting to an HMI.

```iecst
stWarnings : ARRAY [0..GVL.MLX_UBOUND] OF MceWarningsIO; // data for alarm handling of a MotoLogix system
```

Create the *instances*:

```iecst
FB_MceWarnings : ARRAY[0..GVL.MLX_UBOUND] OF MceWarnings;
```

Set the poll interval:

```iecst
GVL.stWarnings[0].tPollInterval := ...;
```

Call the instances in a loop:

```iecst
// function call
FOR i := 0 TO GVL.MLX_UBOUND DO
  FB_Warnings[i](
    io := GVL.stWarnings[i],
    MLX := GVL.stMLX[i]);
END_FOR;
```

Map *all relevant outputs* (see {{< link "MceWarningsIO" >}})
to your HMI:

```iecst
// this is just a portion of the relevant signals
... := GVL.stWarnings[0].aWarnings;
```
