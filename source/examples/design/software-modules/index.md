---
title: Software modules
description: Software modules for the example application.
date: 2022-02-21T17:01:31+02:00
weight: -800
---

{{< lead />}}

In this section we define the modules for the MotoLogix example
application. This helps to create some structure in the program and makes it
easier to understand the relations between the various program parts.
We can distinguish between *common modules* and *application modules*.

## Common modules

The common modules are designed to handle basic MotoLogix tasks and
run independently of the machine's operating mode.
There is no relation between the common modules.

<div class="flex mb-8">
<svg xmlns="http://www.w3.org/2000/svg" width="603" height="203" viewBox="0 0 603 203">

<g stroke-width="2" stroke="currentColor" fill="none">
  <rect width="160" height="80" x="1" y="1" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="81" y="32">Robot alarms</text>
    <text x="81" y="60" class="text-xs italic">MceAlarms</text>
  </g>

  <rect width="160" height="80" x="201" y="1" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="281" y="32">Robot warnings</text>
    <text x="281" y="60" class="text-xs italic">MceWarnings</text>
  </g>

  <rect width="160" height="80" x="401" y="1" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="481" y="32">Actual positions</text>
    <text x="481" y="60" class="text-xs italic">MceActualPositions</text>
  </g>

  <rect width="160" height="80" x="41" y="121" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="121" y="152">Description</text> -->
    <!-- <text x="121" y="180" class="text-xs italic">[function]</text> -->
  </g>

  <rect width="160" height="80" x="241" y="121" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="321" y="152">Description</text> -->
    <!-- <text x="321" y="180" class="text-xs italic">[function]</text> -->
  </g>

  <rect width="160" height="80" x="441" y="121" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="521" y="152">Description</text> -->
    <!-- <text x="521" y="180" class="text-xs italic">[function]</text> -->
  </g>
</g>

</svg>
</div>

- {{< list-link "MceAlarms" >}}
- {{< list-link "MceWarnings" >}}
- {{< list-link "MceActualPositions" >}}

## Application modules

By defining the application modules (and the relation between them) we create
the architecture of the application software.

<div class="flex mb-8">
<svg xmlns="http://www.w3.org/2000/svg" width="890" height="442" viewBox="0 0 890 442">

<g stroke-width="2" stroke="currentColor" fill="none">
  <g>
    <path d="M282 81v20h100v20 M181 121v-20h101V81M181 201v20H81v20 M281
    241v-20H181v-20 M482 241v-20H181v-20 M582 121v-20H282V81 M342 381v-40
    h-61v-20M342 381v-40h140v-20 M582 201v180M582 201v160h199v20M582 201v
    160H422v20 "/>
  </g>

  <rect width="160" height="80" x="201" y="1" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="281" y="32">Operating mode</text>
    <!-- <text x="281" y="60" class="text-xs italic">[function name]</text> -->
  </g>

  <rect width="160" height="80" x="101" y="121" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="180" y="152">Description</text> -->
    <!-- <text x="180" y="180" class="text-xs italic">[function name]</text> -->
  </g>

  <rect width="160" height="80" x="302" y="121" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="381" y="152">Start/stop robot</text>
    <text x="381" y="180" class="text-xs italic">MceStartStop</text>
  </g>

  <rect width="160" height="80" x="501" y="121" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="581" y="152">Manual motion</text>
    <text x="581" y="180" class="text-xs italic">MceManualMotion</text>
  </g>

  <rect width="160" height="80" x="1" y="241" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="80" y="272">Description</text> -->
    <!-- <text x="80" y="300" class="text-xs italic">[function name]</text> -->
  </g>

  <rect width="160" height="80" x="201" y="241" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="280" y="272">Description</text> -->
    <!-- <text x="280" y="300" class="text-xs italic">[function name]</text> -->
  </g>

  <rect width="160" height="80" x="401" y="241" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="481" y="272">Description</text> -->
    <!-- <text x="481" y="300" class="text-xs italic">[function name]</text> -->
  </g>

  <rect width="160" height="80" x="301" y="381" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="381" y="412">PosTable motion</text> -->
    <!-- <text x="381" y="440" class="text-xs italic">McePosTable</text> -->
  </g>

  <rect width="160" height="80" x="501" y="381" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="581" y="412">Jog axes</text>
    <text x="581" y="440" class="text-xs italic">MceRelativeAxisMotions</text>
  </g>

  <rect width="160" height="80" x="701" y="381" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="781" y="412">Jog TCP</text>
    <text x="781" y="440" class="text-xs italic">MceRelativeTcpMotions</text>
  </g>
</g>
</svg>
</div>

- {{< list-link "MceStartStop" >}}
- {{< list-link "MceManualMotion" >}}
- {{< list-link "MceRelativeAxisMotions" >}}
- {{< list-link "MceRelativeTcpMotions" >}}
