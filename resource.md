---
layout: page
title: Resources
permalink: /resources/
---

# Research skills
* [How to Read a Paper](http://ccr.sigcomm.org/online/files/p83-keshavA.pdf)<br>
Article by S. Keshav, July 2007.<br>
[This short paper](/papers/Keshav_HowToReadPaper_Sigcom2007.pdf) helps me read a paper fast and get a critical view as a reviewer. 

* [How (and How Not) to Write  a Good Systems Paper](https://www.usenix.org/conferences/author-resources/how-and-how-not-write-good-systems-paper)<br>
Roy Levin and David D. Redell, Juyly 1983

----------
# Soft skills


----------
# Blog Posts
<ul class="post-list">
  {% for post in site.posts %}
    <li>
      {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
      <span class="post-meta">{{ post.date | date: date_format }}</span>

      <h2>
        <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </h2>
    </li>
  {% endfor %}
</ul>

----------
# Elsewhere links
* [SUTD's site](https://istd.sutd.edu.sg/people/phd-students/ngo-van-mao)

