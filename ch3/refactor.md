# News page refactoring

To apply the DRY principle, I refactored the news page.
Before refactoring the news page has 2 html files, one for displaying news main page, the other for displaying a single news in the same layout as news main page. These isn't inheritance between these 2 pages.

I start by creating a index page:

---html
---
layout: news
title: news
permalink: /news/
author: all
---
{% for post in site.posts %}
	{% include news_item.html  %}
{% endfor %}
---

This is the start point of news main page. It defines what layout will be used and what is the URL to access the news page.

Below the news layout, it will be used in news main page and also in single news page.

---html
---
layout: default
---

<div class="page-header">
  <h1>Les Actualités du Master<small></small></h1>
</div>

<div class="container" style="padding-top:30px">
  <div class="row">
  	<div class="col-md-9">
    
    {{ content }}

  	</div>

   <div class="col-md-3">
    <div class="well">
      <div>
        <ul class="nav nav-stacked">
          <li><a href="{{ link-lang }}/news/">{% t news.allNews %}</a></li>
          <li><a href="{{ link-lang }}/news/1">{% t news.categoryWebsite %}</a></li>
          <li><a href="{{ link-lang }}/news/2">{% t news.categoryInternship %}</a></li>
          <li><a href="{{ link-lang }}/news/3">{% t news.category3 %}</a></li>
        </ul>
        <hr style="border:1px solid black">
        <ul class="nav nav-stacked">
          {% for post in site.posts %}
          {% if post.tag == 'tag1' %}
          <li><a href="{{ link-lang }}{{ post.url }}">{% t news.tag1 %}</a></li>
          {% endif %}
          {% endfor %}
          {% for post in site.posts %}
          {% if post.tag == 'tag2' %}
          <li><a href="{{ link-lang }}{{ post.url }}">{% t news.tag2 %}</a></li>
          {% endif %}
          {% endfor %}        
        </ul>
      </div>

    </div>
  </div>
  </div>
</div>
---

Here, the `{{ content }}` equals to:
---
{% for post in site.posts %}
  {% include news_item.html %}
{% endfor %}
---

that i wrote in the index.html for news main page.

Here is the `news_item.html` file which is included:

---html
<div class="well">
<div style="padding-bottom:30px">
<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
</div>

<div style="padding-bottom:30px">
<span class="label label-default" style="font-size: 150%">{{ post.categories }}</span>

<span>{{ post.date | date_to_string }}</span>
{% for author in site.data.authors %}
{% if author.name == post.author %}
{% if author.photo %}
{% assign photo = author.photo %}
{% else %}
{% assign photo = "/images/photos/unknown-id-128x128.png" %}
{% endif %}          
{% endif %}
{% endfor %}
<img src="{{ photo }}" width="24" height="24">
<span>{{ post.author }}</span>


</div>

<div style="font-size:130%">{{ post.content }}</div>

</div>
---

The variable `{{ post.url }}` will guide us to the single news page. In fact the single news page is aslo a layout. So i defined a layout called `news_item.html` in the `_layout` folder.

---html
---
layout: news
---

<div class="well">
<div style="padding-bottom:30px">
  <h2>{{ page.title }}</h2>
  </div>

  <div style="padding-bottom:30px">
  <span class="label label-default" style="font-size: 150%">{{ page.categories }}</span>

  <span>{{ page.date | date_to_string }}</span>
  {% for author in site.data.authors %}
    {% if author.name == page.author %}
      {% if author.photo %}
      {% assign photo = author.photo %}
      {% else %}
      {% assign photo = "/images/photos/unknown-id-128x128.png" %}
      {% endif %}          
      {% endif %}
  {% endfor %}
  <img src="{{ photo }}" width="24" height="24">
  <span>{{ page.author }}</span>

  
  </div>

  <div style="font-size:130%">{{ content }}</div>

</div>

---

It inherits the news layout and the most imporatnt part is that it was "called" in all the posts.

---
---
layout: news_item
title: Soutenances de stage de M1 et M2
date: 2015-07-17
author: Admin
categories: internship
tag: tag2
---

Les soutenances de stages de M1 et M2 du Master auront lieu respectivement les 26 et 27 août prochains au matin voire début d'après-midi

---

The layout must be specified as `news_item` so that the single news layout will work as intended.