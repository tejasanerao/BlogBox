---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About Me**

Hi I am **{{ site.author.name }}** :wave: :smile:,<br>
Practical and Versatile Computer Engineer working with Web Application Security, Penetration Testing, and Software Development. Certified eLearnSecurity Junior Penetration Tester and looking forward to being Offensive Security Certified Professional(OSCP). Possesses a good understanding of Linux Systems, Database Management Systems, and Computer Networks.

<div style="text-align: center">
<h1>Major Certifications</h1>
</div>

<div class="row">
{% include about/certifications.html %}
</div>

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

## Education

<div class="row">
{% include about/timeline.html %}
</div>
