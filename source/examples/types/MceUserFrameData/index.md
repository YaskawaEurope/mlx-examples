---
title: MceUserFrameData
type: lib
layout: data-type
description: |
  Data type for storing a user frame definition.
  This is a more compact version of MLxAppDataUserFrame.
tags: user frame
categories: examples
---

This defines a user frame as *vector* from world frame to user frame.

![coord-uf](coord-uf.jpg "User frame in relation to the world frame")

Above displayed user frame: `[700, 0, 200, 0, -45, 0]`

| Axis | Value   |
| ---- | ------- |
| X    | 700 mm  |
| Y    | 0 mm    |
| Z    | 200 mm  |
| Rx   | 0 deg   |
| Ry   | -45 deg |
| Rz   | 0 deg   |

{{< note info >}}
User frames which are defined by three taught positions (`ORG/XX/XY`)
can be *transformed* into a vector using matrix calculations or can be *red from
the pendant* (after storing using `MLxWriteApplicationDataToYRC`):
<kbd>robot</kbd> > <kbd>user coordinate</kbd> > <kbd>display</kbd> >
<kbd>coordinate data</kbd>
{{< /note >}}
