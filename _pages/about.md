---
permalink: /
title: "About me"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

I am a PhD student at [Imperial College London](https://www.imperial.ac.uk/space-and-atmospheric-physics) studying the solar wind in the inner heliosphere with the new Parker Solar Probe and Solar Orbiter missions. Interested in using multiple spacecraft to measure the 3D shape of large scale structures in the solar wind, particularly the heliospheric current sheet and stream interaction regions.

> Supported by an Imperial College President's scholarship.

## Publications
<p>
{% for post in site.publications reversed %}
  {% if post.home == true %}
    {% include pub-home.html %}
  {% endif %}
{% endfor %}
</p>

## Education
  * Imperial College London (2019- )
    * PhD in Space Plasma Physics 

  * Imperial College London (2015-2019)
    *  MSci in Physics
    *  Graduated top of the MSci cohort (1st out of 149 students)  

  * Balcarras School (2008-2015)
    * A-level: Physics (A\*), Mathematics (A\*), Further Mathematics (A\*)
    * GCSE: 11 A\*

## Awards
  * Abdus Salam UG Prize for the best MSci graduate in the Physics Department 
  * Governors’ MSci Prize for the best MSci student in Physics at the final examinations
  * Ludlam Prize for excellence in Atmospheric physics 
  * TTP UG Prize for Academic Excellence in Physics for the 2017-2018 academic year 
  * Winton Capital Prize for outstanding performance in second year physics for the 2016-2017 academic year

