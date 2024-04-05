---
title:  
layout: archive
permalink: /
author_profile: true
comments: false
---

<ul>
  <h2>Recent Posts</h2>
  {% capture written_year %}'None'{% endcapture %}
  {% for post in site.posts %}
    {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
    {% if year != written_year %}
      <h3 id="{{ year | slugify }}" class="archive__subtitle">{{ year }}</h3>
      {% capture written_year %}{{ year }}{% endcapture %}
    {% endif %}
    {% include archive-single.html %}
  {% endfor %}
</ul>
