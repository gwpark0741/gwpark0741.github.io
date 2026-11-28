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
  .post article { font-size: 0.9rem; line-height: 1.6; }
  .proj-section { padding-top: 1.75rem; }
  .proj-section h2 { font-size: 1.25rem; font-weight: 600; margin: 0 0 0.25rem; }
  .proj-list { list-style: none; margin: 0; padding: 0; }
  .proj-item { display: grid; grid-template-columns: 200px minmax(0, 1fr); gap: 1.25rem; padding: 1.25rem 0; border-top: 1px solid var(--global-divider-color); }
  .proj-item:first-child { border-top: 0; }
  .proj-thumb { display: flex; align-items: center; justify-content: center; aspect-ratio: 4 / 3; border-radius: 6px; overflow: hidden; border: 1px solid var(--global-divider-color); background: color-mix(in srgb, var(--global-theme-color) 6%, transparent); color: var(--global-text-color-light); font-size: 1.5rem; }
  .proj-thumb img { width: 100%; height: 100%; object-fit: cover; }
  .proj-thumb:focus-visible, .proj-title a:focus-visible { outline: 2px solid var(--global-theme-color); outline-offset: 2px; }
  .proj-head { display: flex; justify-content: space-between; align-items: baseline; gap: 1rem; }
  .proj-title { font-size: 1.05rem; font-weight: 600; }
  .proj-title a { color: inherit; text-decoration: none; }
  .proj-title a:hover { color: var(--global-theme-color); }
  .proj-period { flex: none; font-size: 0.8rem; color: var(--global-text-color-light); font-variant-numeric: tabular-nums; }
  .proj-desc { margin: 0.3rem 0 0.5rem; }
  .proj-foot { display: flex; flex-wrap: wrap; align-items: center; gap: 0.4rem 0.75rem; font-size: 0.8rem; color: var(--global-text-color-light); }
  .proj-badge { padding: 0.1rem 0.55rem; border-radius: 999px; font-weight: 500; color: var(--global-theme-color); background: color-mix(in srgb, var(--global-theme-color) 12%, transparent); }
  @media (max-width: 575px) {
    .proj-item { grid-template-columns: 1fr; gap: 0.75rem; }
    .proj-head { flex-direction: column; gap: 0.1rem; }
  }
</style>

{% for category in page.display_categories %}
{% assign projects = site.projects | where: "category", category | sort: "importance" %}
<section class="proj-section">
  <h2>{% if category == "research" %}Research{% else %}Coursework{% endif %}</h2>
  <ul class="proj-list">
    {% for project in projects %}
    <li class="proj-item">
      <a class="proj-thumb" href="{{ project.url | relative_url }}" aria-label="{{ project.title }}" tabindex="-1">
        {% if project.img %}<img src="{{ project.img | relative_url }}" alt="" loading="lazy">{% else %}<i class="fa-regular fa-image" aria-hidden="true"></i>{% endif %}
      </a>
      <div>
        <div class="proj-head">
          <div class="proj-title"><a href="{{ project.url | relative_url }}">{{ project.title }}</a></div>
          {% if project.period %}<div class="proj-period">{{ project.period }}</div>{% endif %}
        </div>
        <p class="proj-desc">{{ project.description }}</p>
        <div class="proj-foot">
          {% if project.highlight %}<span class="proj-badge">{{ project.highlight }}</span>{% endif %}
          {% if project.context %}<span>{{ project.context }}</span>{% endif %}
        </div>
      </div>
    </li>
    {% endfor %}
  </ul>
</section>
{% endfor %}
