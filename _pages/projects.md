---
layout: page
title: Projects
permalink: /projects/
description: A scrollable view of my research program, from Arctic cloud states and climate intervention to particle-scale mixing and precipitation formation.
nav: true
nav_order: 2
wide_page: true
---

<div class="research-feed-intro">
  <p>
    My research connects particle-scale cloud physics with cloud evolution, precipitation, radiation, and
    climate. The sections below approach that connection through cloud regimes, deliberate intervention,
    mixing, rain formation, and model development.
  </p>
</div>

{% assign research_projects = site.projects | where: "category", "research" | sort: "importance" %}

<nav class="research-feed-toc" aria-label="Research threads">
  <p class="section-kicker">On this page</p>
  <div class="research-feed-toc-links">
    {% for project in research_projects %}
      <a href="#{{ project.title | slugify }}">
        <span>0{{ forloop.index }}</span>
        {{ project.title }}
      </a>
    {% endfor %}
    <a href="#model-development">
      <span>05</span>
      Model development
    </a>
  </div>
</nav>

<div class="research-feed">
  {% for project in research_projects %}
    <section class="research-feed-item research-project-item" id="{{ project.title | slugify }}">
      <div class="research-feed-summary">
        <figure class="research-feed-visual">
          <img src="{{ project.img | relative_url }}" alt="{{ project.title }} research visual" loading="lazy">
        </figure>
        <header class="research-feed-copy">
          <p class="section-kicker">Research thread · 0{{ forloop.index }}</p>
          <h2>{{ project.title }}</h2>
          <p class="research-feed-lede">{{ project.description }}</p>
          <p class="research-feed-context">{{ project.summary }}</p>
        </header>
      </div>
      <div class="research-feed-body">
        {{ project.content | markdownify }}
      </div>
    </section>
  {% endfor %}

  <section class="research-feed-item" id="model-development">
    <div class="research-feed-summary">
      <figure class="research-feed-visual">
        <img
          src="{{ '/assets/img/droplab_deep_convection.gif' | relative_url }}"
          alt="Particle-based deep-convection simulation in DropLab"
          loading="lazy"
        >
        <figcaption>Particle-based deep-convection experiment in DropLab</figcaption>
      </figure>
      <header class="research-feed-copy">
        <p class="section-kicker">Research infrastructure · 05</p>
        <h2>Model development</h2>
        <p class="research-feed-lede">
          I develop cloud models and research software for studying particle-scale physics in idealized and
          high-resolution simulations.
        </p>
      </header>
    </div>
    <div class="research-feed-body">
      <div class="research-detail">
        <h3>DropLab development</h3>
        <p>
          I develop <a href="{{ '/droplab/' | relative_url }}">DropLab</a>, an open-source Lagrangian cloud
          laboratory for research exploration and teaching. It connects individual super-droplet histories
          to parcel, cloud-deck, and deep-convection experiments that can run on a laptop.
        </p>
      </div>

      <div class="research-detail">
        <h3>GPU-accelerated particle-based microphysics in LES</h3>
        <p>
          I am developing GPU-accelerated particle-based microphysics for the Lagrangian Cloud Model (LCM),
          coupled to the
          <a href="http://rossby.msrc.sunysb.edu/SAM.html" target="_blank" rel="noopener">System for Atmospheric Modeling (SAM)</a>,
          using OpenACC on NVIDIA GPUs. Ongoing end-to-end benchmarks show <strong>17–38× speedups</strong>
          across the cases tested so far; validation and performance benchmarking are ongoing.
        </p>
      </div>
    </div>

  </section>
</div>
