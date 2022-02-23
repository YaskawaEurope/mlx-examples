---
title: Style guide
description: Programming style guidelines for the MotoLogix code examples.
date: 2022-02-21T17:38:30+02:00
weight: -900
---

{{< lead />}}

The goal of these guidelines is to make the code more uniform and
improve its readability.

## Programming conventions

1. All programming in *Structured Text* (`ST`).
1. Variable names starts with a [prefix](#naming-conventions).
1. Function parameters do *not* use a prefix.
1. Constants are written in `UPPERCASE`, use the underscore (`_`) as
   separator and do *not* use a prefix.
1. Focus on platform compatibility by favouring plain/universal code over
   platform specific functions.
1. Wrap program code lines at max `80 characters`.
1. Tabs as `spaces`.
1. Tab width: `2 characters`.
1. No trailing whitespace.

## Coding reference

### One shot signals

{{< note >}}
For compatibility reasons we do not use platform specific functions such
as `R_TRIG`.
{{< /note >}}

| Name/prefix | Description                                    |
| ----------- | ---------------------------------------------- |
| `aOneShots` | name of the array for storing one shot signals |
| `bOsr`      | prefix for a rising edge                       |
| `bOsf`      | prefix for a falling edge                      |

```iecst
// -----------------------------------------------------------------------------
// declaration
// -----------------------------------------------------------------------------
bMyInput: BOOL; // input for testing the one shot logic
aOneShots: ARRAY [0..9] OF BOOL; // bits for creating one shot signals
bOsrMyInput: BOOL; // rising edge for MyInput
bOsfMyInput: BOOL; // falling edge for MyInput


// -----------------------------------------------------------------------------
// logic
// -----------------------------------------------------------------------------
// rising edge
bOsrMyInput := bMyInput AND NOT aOneShots[0];
aOneShots[0] := bMyInput;

// falling edge
bOsfMyInput := NOT bMyInput AND NOT aOneShots[1];
aOneShots[1] := NOT bMyInput;
```

## Naming conventions

{{< note >}}
This is based on
[Beckhoff's](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_plc_intro/3363785099.html)
TwinCAT3 programming conventions.
{{< /note >}}

### Variables

Below table lists the prefix for each data type.

| Data type            | Prefix |
| -------------------- | ------ |
| FUNCTION_BLOCK       | `fb`   |
| STRUCT               | `st`   |
| ARRAY                | `a`    |
| ENUM                 | `e`    |
| INTERFACE            | `ip`   |
| UNION                | `u`    |
| BOOL                 | `b`    |
| BYTE                 | `n`    |
| WORD                 | `n`    |
| DWORD                | `n`    |
| LWORD                | `n`    |
| SINT                 | `n`    |
| USINT                | `n`    |
| INT                  | `n`    |
| UINT                 | `n`    |
| DINT                 | `n`    |
| UDINT                | `n`    |
| LINT                 | `n`    |
| ULINT                | `n`    |
| REAL                 | `f`    |
| LREAL                | `f`    |
| STRING               | `s`    |
| WSTRING              | `s`    |
| TIME                 | `t`    |
| LTIME                | `t`    |
| TIME_OF_DAY          | `td`   |
| DATE_AND_TIME        | `dt`   |
| DATE                 | `d`    |
| POINTER              | `p`    |
| POINTER TO INTERFACE | `pip`  |
| POINTER TO POINTER   | `pp`   |
| REFERENCE TO         | `ref`  |
