---
layout: archive
title: ""
author_profile: false
sidebar: false
---

<style>
  .page__content,
  .archive {
    max-width: 1000px;
    margin: 0 auto;
    padding: 1em;
  }

  .page,
  #main {
    display: flex;
    justify-content: center;
    width: 100%;
  }

  .archive {
    width: 100%;
  }
</style>

<div class="page__content">
  <p style="font-size: 1.1em;">
      </p>

  <h2>Recent Publications</h2>

  <ul>
    {% assign first_author_pubs = site.publications | where: "category", "manuscripts" | sort: "date" | reverse | slice: 0, 3 %}
    {% for pub in first_author_pubs %}
      <li style="margin-bottom: 1.5em;">
        <strong><a href="{{ pub.url }}">{{ pub.title }}</a></strong><br>
        <em>{{ pub.authors | join: ", " }}</em><br>
        <span>{{ pub.content | strip_html | truncatewords: 30 }}</span>
      </li>
    {% endfor %}
  </ul>

  <p><a href="/publications/">More publications →</a></p>
</div>
