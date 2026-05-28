---
layout: page
permalink: /publications/
title: Publications
description: Peer-reviewed publications in cloud microphysics and atmospheric science.
nav: true
nav_order: 3
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

<h2 class="pub-section">In Preparation and Under Review</h2>
<div class="starred-bib">
{% bibliography --group_by none --query @unpublished* %}
</div>

<h2 class="pub-section">Published</h2>
{% capture pub_count_raw %}{% bibliography_count --query @article* %}{% endcapture %}
{% assign pub_start = pub_count_raw | strip | plus: 1 %}
<div class="numbered-bib" style="--pub-start: {{ pub_start }};">
{% bibliography --query @article* %}
</div>

</div>
