---
layout: page
title: projects
permalink: /projects/
description: Research and course projects.
nav: true
nav_order: 1
display_categories: [research, coursework]
---

<!-- 프로젝트 목록. 각 프로젝트는 _projects/*.md 파일 하나입니다.
     왼쪽 사진: 그 파일 front matter 에 img: /assets/img/projects/파일명.jpg 를 넣으면 나타납니다. 없으면 빈 칸이 보입니다. -->

<style>
  .post article { font-size: 0.875rem; line-height: 1.6; }
  .proj-section { padding-top: 1.5rem; }
  .proj-section h2 { font-size: 0.8rem; font-weight: 500; letter-spacing: 0.08em; text-transform: uppercase; margin: 0 0 1rem; }
  .proj-list { list-style: none; margin: 0; padding: 0; }
  .proj-item { display: flex; gap: 1.25rem; padding: 1rem 0; border-top: 1px solid rgba(128, 128, 128, 0.25); }
  .proj-item:first-child { border-top: 0; padding-top: 0; }
  .proj-thumb { flex: none; width: 180px; aspect-ratio: 4 / 3; border-radius: 0.25rem; overflow: hidden; background: rgba(128, 128, 128, 0.12); display: flex; align-items: center; justify-content: center; color: rgba(128, 128, 128, 0.6); font-size: 1.5rem; }
  .proj-thumb img { width: 100%; height: 100%; object-fit: cover; }
  .proj-body { flex: 1; min-width: 0; }
  .proj-title { font-size: 1rem; font-weight: 500; }
  .proj-title a { color: inherit; text-decoration: none; }
  .proj-title a:hover { text-decoration: underline; }
  .proj-meta { font-size: 0.75rem; opacity: 0.65; margin: 0.1rem 0 0.35rem; }
  .proj-desc { margin: 0; }
  @media (max-width: 575px) {
    .proj-item { flex-direction: column; gap: 0.6rem; }
    .proj-thumb { width: 100%; }
  }
</style>

{% for category in page.display_categories %}
{% assign projects = site.projects | where: "category", category | sort: "importance" %}
<section class="proj-section">
  <h2>{% if category == "research" %}Research &amp; Projects{% else %}{{ category }}{% endif %}</h2>
  <ul class="proj-list">
    {% for project in projects %}
    <li class="proj-item">
      <a class="proj-thumb" href="{{ project.url | relative_url }}" aria-label="{{ project.title }}">
        {% if project.img %}<img src="{{ project.img | relative_url }}" alt="{{ project.title }}" loading="lazy">{% else %}<i class="fa-regular fa-image"></i>{% endif %}
      </a>
      <div class="proj-body">
        <div class="proj-title"><a href="{{ project.url | relative_url }}">{{ project.title }}</a></div>
        {% if project.meta %}<div class="proj-meta">{{ project.meta }}</div>{% endif %}
        <p class="proj-desc">{{ project.description }}</p>
      </div>
    </li>
    {% endfor %}
  </ul>
</section>
{% endfor %}
