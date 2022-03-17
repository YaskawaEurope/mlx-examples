---
title: MceRelativeTcpMotions
type: lib
layout: function
description: |
  Create a flow of relative TCP motion commands.
  Useful for jogging the TCP.
tags:
  - jog
categories: examples
---

The state machine handles three instances of the relative
axis motion command (`MLxRobotMoveAxisRelative`).

- As long as motion is requested (`aDirections[] <> 0`), the state machine starts
  looping in state `10/20/30` and initiating incremental motion commands.
- When motion is no longer requested (`aDirections[] = 0`) the state machine
  will keep looping until all pre-sent motions are finished and then return to
  the idle state.
  The time this takes (*coast time*) depends on the combination of speed
  and delta.

  For a good user experience, the coast time should be as short as possible.
  This can be achieved by optimizing the motion deltas, see this
  {{< link "Relative motion deltas" "tech article">}}.

It uses the principle where each state has a *primary motion* and sends up to
*two motions in advance*.
This way the robot controller always has the required three motions (for
smooth blending) in its motion queue.

```iecst
// start this motion
fbMove[0].Enable := TRUE;

// queue next 2 motions
fbMove[1].Enable := (fbMove[0].Sts_EN AND fbMove[0].Sts_DN) AND (nQa >= 1);
fbMove[2].Enable := (fbMove[1].Sts_EN AND fbMove[1].Sts_DN) AND (nQa >= 2);
```

- By using the `Sts_DN` of the preceding command the commands are
  sent nicely after each other.

- The QA variable (*Queueing Amount*) is used as mechanism to work towards
  a standstill.
  In such case the state machine should no longer keep sending motions in
  advance but it should still handle the already sent motions.

The QA is being recalculated at every transition between these three states.
A transition takes place when the primary motion is finished (`Sts_PC`).

```iecst
IF fbMove[0].Sts_EN AND fbMove[0].Sts_PC THEN
  fbMove[0].Enable := FALSE;

  // calc QA
  IF bRun AND NOT io.bInchingMode THEN
    nQa := USINT_TO_SINT(io.nQaMax); // no standstill foreseen
  ELSE
    nQa := nQa - 1; // planning towards standstill
  END_IF;

  IF (nQa >= 0) THEN
    // continue
    io.nSmMotions := 20;
  ELSE
    // all motions done
    io.nSmMotions := 40;
  END_IF;
END_IF;
```

## Usage

{{< note >}}
If you want to create a jog program please have a look at
{{< link MceManualMotion >}} which already implements this function.
{{< /note >}}