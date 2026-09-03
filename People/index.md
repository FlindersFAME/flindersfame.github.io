---
layout: splash
permalink: /people/
title: FAME-ous people
author_profile: true
ptags:
  - Group Leaders
  - Researchers
  - Students
  - Visiting Scholars
  - Alumni
---

# FAME-ous People!

![](/assets/images/FAME_lab_photo_2026.jpg)

{% for ptag in page.ptags %}
## {{ ptag }}
  {% assign sortedPosts = site.categories.People | sort: 'title' %}
  {% for post in sortedPosts %}
    {% if post.tags contains ptag %}
<div class="author__avatar">
  <img src="{{ site.data.authors[post.author].avatar }}" style="float: left; margin-right: 20pt;">
</div>
<a href="{{ post.url }}">{{ post.title }}</a><br>
{{ post.excerpt }}
<br>
    {% endif %}
  {% endfor %}
{% endfor %}

## Previous lab photos

### 2022 - 2025

![](/assets/images/FAME_lab_photo_2022.jpg)