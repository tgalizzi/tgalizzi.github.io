---
layout: page
permalink: /formation/
title: Formation
description: Double diplomé en informatique et mathématiques appliquées.
years: [Georgia Tech, ENSEEIHT]
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% if y == "ENSEEIHT" %}
  <h2>Informatique & Mathématiques appliquées</h2>
  <p>Diplôme d'ingénieur, spécialisation High Perfomance Computing & Big Data</p>

  {% endif %}
  {% if y == "Georgia Tech" %}
  <h2>College of Computing</h2>
  <p>Master of science en computer science, spécialisation Machine Learning </p>

  {% endif %}
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
