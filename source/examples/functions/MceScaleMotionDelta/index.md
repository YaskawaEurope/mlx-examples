---
title: MceScaleMotionDelta
type: lib
layout: function
description: |
  Calculate the travel distance per relative motion
  command for a certain speed by scaling between delta values of low and
  high speed.
tags:
  - jog
  - endless rotation
---

{{< note >}}
Check this {{< link "Relative motion deltas" "tech article">}} about relative
motion deltas.
{{< /note >}}

## Usage

Determined deltas for relative axis motion:

```iecst
// determined values at low speed
io.stMotionDeltas.stAxis.fSpeedLo := 0.5;
io.stMotionDeltas.stAxis.aDeltaLo[0] := 0.1;
io.stMotionDeltas.stAxis.aDeltaLo[1] := 0.1;
io.stMotionDeltas.stAxis.aDeltaLo[2] := 0.1;
io.stMotionDeltas.stAxis.aDeltaLo[3] := 0.2;
io.stMotionDeltas.stAxis.aDeltaLo[4] := 0.2;
io.stMotionDeltas.stAxis.aDeltaLo[5] := 0.2;

// determined values at high speed
io.stMotionDeltas.stAxis.fSpeedHi := 30;
io.stMotionDeltas.stAxis.aDeltaHi[0] := 7;
io.stMotionDeltas.stAxis.aDeltaHi[1] := 7;
io.stMotionDeltas.stAxis.aDeltaHi[2] := 7;
io.stMotionDeltas.stAxis.aDeltaHi[3] := 8;
io.stMotionDeltas.stAxis.aDeltaHi[4] := 8;
io.stMotionDeltas.stAxis.aDeltaHi[5] := 12;
```

Calculate the deltas for an instance of {{< link MceRelativeAxisMotions >}}.
In case of inching mode a fixed distance and speed is used.
Otherwise the deltas are scaled based on the speed setting:

```iecst
IF io.bInchingMode THEN
  // fixed distance and speed
  stJogAxis.fSpeed := 10; // %
  FOR i := 0 TO 5 DO
    stJogAxis.aDelta[i] := 0.1; // deg
  END_FOR;
ELSE
  // distance based on speed
  stJogAxis.fSpeed := io.fSpeed;
  stJogAxis.aDelta :=
    MceScaleMotionDelta(
      speed := io.fSpeed,
      data := io.stMotionDeltas.stAxis,
      factor := 1 );
END_IF;
```
