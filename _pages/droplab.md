---
layout: page
title: DropLab
description: An open-source Lagrangian cloud laboratory—from warm-rain parcels to mixed-phase convection
permalink: /droplab/
nav: true
nav_order: 3
---

<figure class="project-lead-visual">
  <img src="{{ '/assets/img/droplab_app.png' | relative_url }}" alt="DropLab sandbox home screen">
  <figcaption>The DropLab sandbox organizes cloud experiments into Parcel, 2-D Cloud, Climate, and Lecture modes.</figcaption>
</figure>

**DropLab** is an open-source Lagrangian super-droplet laboratory for cloud microphysics. Aerosol
particles, cloud droplets, and ice crystals are represented by simulated super-droplets, making
particle histories visible while keeping curated experiments fast enough to run on a laptop.

### What you can explore

- aerosol activation, condensation, collision–coalescence, and warm-rain initiation
- turbulent entrainment and droplet-size-distribution evolution
- Arctic mixed-phase clouds, depositional ice growth, riming, melting, and crystal habits
- idealized 2-D shallow clouds and anelastic deep convection
- marine cloud brightening and glaciogenic-seeding demonstrations
- guided predict–observe–explain lessons for university teaching

### Four modes

1. **Parcel** — follow a rising air parcel from aerosol activation toward precipitation
2. **2-D Cloud** — watch warm, mixed-phase, or deep-convective clouds evolve
3. **Climate** — compare baseline and idealized intervention experiments
4. **Lecture** — use curated lessons built around prediction and explanation

### Validation and scope

DropLab's microphysical kernels are tested against analytic solutions and reference Fortran
implementations. Its integrated 2-D cloud experiments remain idealized: they are designed for process
exploration and teaching, not forecasting. The repository documents those limits alongside the
validation suite.

<div class="project-actions">
  <a class="feature-primary" href="https://github.com/jslim93/DropLab" target="_blank" rel="noopener">Get DropLab on GitHub</a>
  <a class="feature-secondary" href="https://github.com/jslim93/DropLab#how-to-run" target="_blank" rel="noopener">Run the sandbox</a>
</div>
