---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  <div style="text-align: center; margin-bottom: 2em; margin-top: 1em;">
    <a href="{{ author.googlescholar }}" class="btn btn--info btn--large"><i class="fas fa-fw fa-graduation-cap"></i> View my Google Scholar Profile</a>
  </div>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
