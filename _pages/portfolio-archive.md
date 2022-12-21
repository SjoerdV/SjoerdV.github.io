---
layout: archive
title: "Portfolio"
permalink: /portfolio/
author_profile: true
entries_layout: grid
classes: single
---

{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.portfolio %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
