---
layout: archive
title: "Updates"
permalink: /updates/
redirect_from:
  - /news/
  - /news.html
author_profile: false
---

A reverse-chronological log of talks, fellowships, recognitions, and announcements &mdash; including selected LinkedIn highlights.

{% assign sorted_news = site.data.news | sort: 'date' | reverse %}
{% assign featured = sorted_news | where: 'featured', true %}

{% if featured.size > 0 %}
<section class="featured-news">
  <h2 class="research-section">Featured</h2>
  <div class="featured-news__grid">
    {% for item in featured %}
      {% if item.link %}<a class="featured-card" href="{{ item.link }}" target="_blank" rel="noopener">{% else %}<article class="featured-card">{% endif %}
        <p class="featured-card__when">{{ item.date | date: "%b %Y" }}</p>
        <h3 class="featured-card__title">{{ item.title }}</h3>
        <p class="featured-card__body">{{ item.body }}</p>
      {% if item.link %}</a>{% else %}</article>{% endif %}
    {% endfor %}
  </div>
</section>
{% endif %}

<h2 class="research-section">Timeline</h2>

{% assign by_year = sorted_news | group_by_exp: 'item', 'item.date | date: "%Y"' %}

<div class="year-tabs" data-year-tabs>
  <div class="year-tabs__nav" role="tablist">
    {% for year_group in by_year %}
      <button type="button" class="year-tabs__tab{% if forloop.first %} is-active{% endif %}" data-year="{{ year_group.name }}" role="tab" aria-selected="{% if forloop.first %}true{% else %}false{% endif %}">{{ year_group.name }}</button>
    {% endfor %}
  </div>

  {% for year_group in by_year %}
    <div class="year-tabs__panel{% if forloop.first %} is-active{% endif %}" data-year="{{ year_group.name }}" role="tabpanel">
      <ul class="news-cards">
        {% for item in year_group.items %}
          <li class="news-card">
            <div class="news-card__meta">
              <p class="news-card__date">{{ item.date | date: "%b %-d" }}</p>
              {% if item.tags contains 'talk' %}<span class="news-card__pill news-card__pill--talk">Talk</span>{% endif %}
            </div>
            <div class="news-card__body">{{ item.html }}</div>
          </li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</div>

<script>
(function(){
  var root = document.querySelector('[data-year-tabs]');
  if (!root) return;
  var tabs = root.querySelectorAll('.year-tabs__tab');
  var panels = root.querySelectorAll('.year-tabs__panel');
  tabs.forEach(function(tab){
    tab.addEventListener('click', function(){
      var year = tab.getAttribute('data-year');
      tabs.forEach(function(t){
        var active = t.getAttribute('data-year') === year;
        t.classList.toggle('is-active', active);
        t.setAttribute('aria-selected', active ? 'true' : 'false');
      });
      panels.forEach(function(p){
        p.classList.toggle('is-active', p.getAttribute('data-year') === year);
      });
    });
  });
})();
</script>
