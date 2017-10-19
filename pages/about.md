---
layout: page
title: 麻吉卡
description: About Me
keywords: Me
comments: true
menu: Me
permalink: /about/
---

麻吉卡

## Contact Me
b00505005@ntu.edu.tw
{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
