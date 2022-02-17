---
title: MceWarnings
type: lib
layout: function
description: |
  Reading and displaying the warnings of a MotoLogix system.
tags: general
categories: examples
---

For this purpose it uses two states machines:

- {{< link "MceWarningsIO#nSmReadWarnings" "nSmReadWarnings">}} - getting the
  warning information from the controller
- {{< link "MceWarningsIO#nSmDisplayWarnings" "nSmDisplayWarnings">}} - display
  warnings one after the other

## Usage

{{< note >}}
Each MotoLogix system needs its own `MceWarnings` instance.
{{< /note >}}

We start by creating the (global) variables for the *interface data*.
These variables are also used for connecting buttons or an HMI.


```iecst
stWarnings : ARRAY [0..GVL.MLX_UBOUND] OF MceWarningsIO; // data for alarm handling of a MotoLogix system
```

Now we create the *instances*:

```iecst
FB_MceWarnings : ARRAY[0..GVL.MLX_UBOUND] OF MceWarnings;
```

Map *all relevant inputs* (see {{< link "MceWarningsIO" >}})
to your HMI.

```iecst
GVL.stWarnings[0].bReset := ...;
```

Since we defined the variables as arrays we can process the *function calls*
in a loop:

```iecst
// function call
FOR i := 0 TO GVL.MLX_UBOUND DO
  FB_Warnings[i](
    io := GVL.stWarnings[i],
    MLX := GVL.stMLX[i]);
END_FOR;
```

Map *all relevant outputs* (see {{< link "MceWarningsIO" >}})
to your HMI and/or to higher level state machines.

```iecst
// this is just a portion of the relevant signals
... := GVL.stWarnings[0].aWarning;
```
