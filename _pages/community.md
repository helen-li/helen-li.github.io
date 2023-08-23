---
layout: page
permalink: /community/
title: community
description: organizing, talking, helping
nav: true
nav_order: 4
display_categories: [education, empowerment, events]
---
<!-- add the most recent events to the bottom of _community for each category -->
<div class="news">
    {% if site.community != blank -%}
      {%- assign news_size = site.community | size -%}
      {%- assign news = site.community | reverse -%}

      {% for category in page.display_categories %}
        <h3 class="category">{{ category }}</h3>
        <div class="table-responsive" {% if include.limit and site.announcements.scrollable and news_size > 3 %}style="max-height: 60vw"{% endif %}>
          <table class="table table-sm table-borderless">
            {%- assign categorized_news = site.community | where: "category", category -%}
            {%- assign sorted_news = categorized_news | sort: "order" | reverse %}
            {% for item in sorted_news %}
              <tr>
                <th scope="row">{{ item.datestring }}</th>
                <td>
                  {% if item.inline -%}
                    {{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
                  {%- else -%}
                    <a class="news-title" href="{{ item.url | relative_url }}">{{ item.title }}</a>
                  {%- endif %}
                </td>
              </tr>
            {%- endfor %}
          </table>
        </div>
      {% endfor %}
    {%- else -%}
      <p>No news so far...</p>
    {%- endif %}
</div>