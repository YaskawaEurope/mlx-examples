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
<svg xmlns="http://www.w3.org/2000/svg" pointer-events="bounding-box" width="603" height="203" viewBox="-1 -1 603 203">

<g stroke-width="2" stroke="currentColor" fill="none">
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="81" y="32">Robot alarms</text>
    <text x="81" y="60" class="text-xs italic">MceAlarms</text>
  </g>
  <a href='{{< ref "MceAlarms" >}}'>
    <rect width="160" height="80" x="1" y="1" rx="10"/>
  </a>

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="281" y="32">Robot warnings</text>
    <text x="281" y="60" class="text-xs italic">MceWarnings</text>
  </g>
  <a href='{{< ref "MceWarnings" >}}'>
    <rect width="160" height="80" x="201" y="1" rx="10"/>
  </a>

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="481" y="32">Description</text> -->
    <!-- <text x="481" y="60" class="text-xs italic">[function]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="401" y="1" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="121" y="152">Description</text> -->
    <!-- <text x="121" y="180" class="text-xs italic">[function]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="41" y="121" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="321" y="152">Description</text> -->
    <!-- <text x="321" y="180" class="text-xs italic">[function]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="241" y="121" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="521" y="152">Description</text> -->
    <!-- <text x="521" y="180" class="text-xs italic">[function]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="441" y="121" rx="10"/>
  <!-- </a> -->
</g>

</svg>
</div>

## Application modules

By defining the application modules (and the relation between them) we create
the architecture of the application software.

<div class="flex mb-8">
<svg xmlns="http://www.w3.org/2000/svg" pointer-events="bounding-box" width="663" height="443" viewBox="-1 -1 663 443">

<g stroke-width="2" stroke="currentColor" fill="none">
  <g>
    <path d="M281 81v20h101v20"/>
    <path d="M181 121v-20h100V81M181 201v20H81v20"/>
    <path d="M281 241v-20H181v-20"/>
    <path d="M481 241v-20H181v-20"/>
    <path d="M581 361V101H281V81"/>
    <path d="M381 361v-20H281v-20m100 40v-20h100v-20"/>
  </g>

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="281" y="32">Operating mode</text>
    <!-- <text x="281" y="60" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="201" y="1" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="180" y="152">Description</text> -->
    <!-- <text x="180" y="180" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="101" y="121" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="381" y="152">Start/stop robot</text>
    <text x="381" y="180" class="text-xs italic">MceStartStop</text>
  </g>
  <a href='{{< ref "MceStartStop" >}}'>
    <rect width="160" height="80" x="302" y="121" rx="10"/>
  </a>

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="80" y="272">Description</text> -->
    <!-- <text x="80" y="300" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="1" y="241" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="280" y="272">Description</text> -->
    <!-- <text x="280" y="300" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="201" y="241" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="481" y="272">Description</text> -->
    <!-- <text x="481" y="300" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="401" y="241" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="581" y="392">Description</text> -->
    <!-- <text x="581" y="420" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="501" y="361" rx="10"/>
  <!-- </a> -->

  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <!-- <text x="381" y="392">Description</text> -->
    <!-- <text x="381" y="420" class="text-xs italic">[function name]</text> -->
  </g>
  <!-- <a href='{{< ref "" >}}'> -->
    <rect width="160" height="80" x="301" y="361" rx="10"/>
  <!-- </a> -->
</g>
</svg>
</div>
