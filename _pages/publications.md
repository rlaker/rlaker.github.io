---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% include base_path %}

<p>
{% for post in site.publications reversed %}
  {% include pub-home.html %}
{% endfor %}
</p>