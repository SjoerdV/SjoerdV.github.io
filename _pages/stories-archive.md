---
layout: archive
title: "Stories"
permalink: /stories/
author_profile: false
entries_layout: grid
classes: single
---

{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.stories %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
