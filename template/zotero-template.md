---
layout: post
title: "{{title | replace('"', '\\"')}}"
date: {{date | format("YYYY-MM-DD HH:mm:ss")}}
description: "Paper review: {{title | replace('"', '\\"')}}"
tags: [paper-review]
categories: [Research]
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

## Overview
- **Title**: {{title}}
- **Authors**: {% for author in creators %}{{author.firstName}} {{author.lastName}}{% if not loop.last %}, {% endif %}{% endfor %}
- **Year**: {{date | format("YYYY")}}
- **Citekey**: `{{citekey}}`
{% if url %}- **Link**: [Paper Link]({{url}}){% endif %}

---

## 1. Goal
*What did the authors try to accomplish? (목표/연구 문제)*



## 2. Approach
*What were the key elements of the approach? (핵심 접근법 및 신규 설계)*



## 3. Takeaways for Me
*What can you use yourself? (내 연구에 활용할 수 있는 점)*



## 4. Follow-up Reading
*What other references do you want to follow? (추가로 읽어볼 인용 논문)*

-
