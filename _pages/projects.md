---
title: Projects
layout: collection
permalink: /projects/
collection: projects
entries_layout: grid
classes: wide
---

{% assign projects = site.projects | sort: "order" %}
{% assign site.projects = projects %}

These are some of the personal projects I have been up to. Check them out on
[GitHub]({{site.github.owner_url}}).
