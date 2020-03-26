---
layout: page
title: About
description: 爱武术，爱编程
keywords: XL,Pegasus
comments: true
menu: 关于
permalink: /about/
---

我是Pegasus，喜欢武术喜欢编程。

## 联系

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
