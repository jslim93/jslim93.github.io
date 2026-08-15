---
layout: page
permalink: /publications/
title: Publications
description: Current manuscripts and peer-reviewed publications in cloud microphysics and atmospheric science.
nav: true
nav_order: 4
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

<h2 class="pub-section">Current Manuscripts</h2>
<div class="manuscript-status-list" aria-label="Current manuscript status">
  <div class="manuscript-status-entry">
    <span><strong>Lim and Feingold</strong></span>
    <span class="manuscript-status">In preparation</span>
  </div>
  <div class="manuscript-status-entry">
    <span><strong>Lim et al.</strong></span>
    <span class="manuscript-status">In preparation</span>
  </div>
</div>

<h2 class="pub-section">Peer-Reviewed Publications</h2>
{% capture pub_count_raw %}{% bibliography_count --query @article* %}{% endcapture %}
{% assign pub_start = pub_count_raw | strip | plus: 1 %}
<div class="numbered-bib" style="--pub-start: {{ pub_start }};">
{% bibliography --query @article* %}
</div>

</div>
