---
layout: single
author_profile: true
permalink: /software/
title: FAME Bioinformatics Software
---

Many Bioinformatics tools for performing a variety of analyses have been created by current and past members of FAME.
Browse our currently-maintained, open-source, and freely available Bioinformatics tools by category below.


{% assign sorted_tags = site.tags | sort %}
 {% for tag in sorted_tags %}
  {% assign zz = tag[1] | where: "category", "software" | sort %}
  {% if zz != empty %}

<h3>{{ tag[0] }}</h3>
<details>
  <summary>
    (<i>Show all {{ tag[0] }} tools</i>)
  </summary>
  <ul>
    {% for post in tag[1] reversed %}
    <li><a href="{{ post.url }}">{{ post.title }}</a>{{ post.excerpt }}</li>
    {% endfor %}
  </ul>
</details>

  {% endif %}
{% endfor %}



