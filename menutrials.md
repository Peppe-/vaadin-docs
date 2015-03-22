---
layout: page
title: Menu Trials
---

  <h2>jekyll page stuff</h2>
  
  {% for section in site.data.docs %}
    <h4>{{ section.title }}</h4>
    {% include docs_chapter.html items=section %}
  {% endfor %}
  <h2>split join loop</h2>
  {% for page in site.pages %}            
    {{ page.url | split:'/' | join:'+'}}
  {% endfor %}
  <h2>site.pages loop</h2>
  <ul>
    {% for p in site.pages %}
      <li>
        <a href="{{ p.url }}">{{ p.title }}</a>
      </li>
    {% endfor %}
  </ul>
  <h2>collection driven</h2>
  <ul>
  {% for page in site.collections.book.docs %}
    <li><a href="{{ page.url }}">{{page.url}} -- {{ page.title }}</a></li>

  {% endfor %}
  </ul>

  <h2>url driven</h2>
  {% assign url_parts = page.url | split: '/' %}
  {% assign url_parts_size = url_parts | size %}
  {% assign rm = url_parts | last %}
  {{ulr_parts}} -- {{url_parts_size}} -- {{page.url}} -- {{rm}}

  <ul>
  {% for node in site.pages %}
      {% assign node_url_parts = node.url | split: '/' %}
      {% assign node_url_parts_size = node_url_parts | size %}
      {% assign filename = node_url_parts | last %}
      <li><a href='{{node.url}}'>{{node.title}}</a></li> -- {{filename}} -- {{ node_url_parts }}
      
    
  {% endfor %}
  </ul>