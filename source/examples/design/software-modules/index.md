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
<svg xmlns="http://www.w3.org/2000/svg" width="526" height="201" viewBox="-.5 -.5 526 201">

<g stroke-width="1" stroke="currentColor" fill="none">

  <rect x="0" y="0" width="150" height="75" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="74.5" y="28">Robot alarms</text>
    <text x="74.5" y="56" class="text-xs italic">MceAlarms</text>
  </g>

  <rect x="175" y="0" width="150" height="75" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
    <text x="249.5" y="28">Robot warnings</text>
    <text x="249.5" y="56" class="text-xs italic">MceWarnings</text>
  </g>

  <rect x="350" y="0" width="150" height="75" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
  </g>

  <rect x="25" y="100" width="150" height="75" rx="10"/>
  <g class="text-sm" text-anchor="middle" stroke="none" fill="currentColor">
  </g>
</g>

</svg>
</div>

