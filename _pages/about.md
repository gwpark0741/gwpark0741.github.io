---
layout: about
title: about
permalink: /
subtitle:

selected_papers: false # Publications 는 아래 본문에서 직접 배치합니다 (순서를 Education 앞으로 두기 위해)
social: false # 연락 아이콘은 아래 본문에서 소개 바로 밑에 직접 배치합니다

announcements:
  enabled: false # News 는 지금 비워 둠. 내용이 생기면 _news/ 에 글을 만들고 true 로 바꾸면 됩니다.
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false # 템플릿 기본 위치(맨 아래)는 끄고, 아래 본문에서 News 밑에 직접 배치합니다. 개수는 limit.
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

<div class="about-top">
<div class="about-text" markdown="1">

Hi, I'm Gwangwoong! I'm an undergraduate student at Sogang University, studying Business Administration and Computer Science and Engineering. Since January 2026, I've been a research intern at the NLP & ISDS Lab.

My research is driven by a simple question: how can we make models genuinely understand? I work on multimodal representation learning and generative models, and recently I've become interested in physical AI, especially world action models.

<div class="social"><div class="contact-icons"><a href="#" id="email-link" data-e="Z3dhbmd3b29uZy5wYXJrQGdtYWlsLmNvbQ==" title="Email"><i class="fa-solid fa-envelope"></i></a> {% social_links %}</div></div>
<script>document.getElementById("email-link").addEventListener("click", function (e) { e.preventDefault(); location.href = "mailto:" + atob(this.dataset.e); });</script>

</div>
<img class="about-photo" src="{{ '/assets/img/prof_pic.jpg' | relative_url }}" alt="Gwangwoong Park"> <!-- 사진 교체: assets/img/prof_pic.jpg 를 덮어쓰면 됩니다 -->
</div>

<hr class="about-break">

<style>
  /* About 페이지 전용 스타일: 본문은 작고 조용하게, 포인트 색은 --about-accent 하나로 조절, 섹션마다 자기 영역을 확보 */
  .post article { --about-accent: #111827; font-size: 0.875rem; line-height: 1.6; }
  [data-theme="dark"] .post article { --about-accent: #e5e7eb; }
  .about-top { display: flex; align-items: stretch; gap: 1.5rem; }
  .about-text { flex: 1; min-width: 0; }
  .about-photo { width: 190px; flex: none; object-fit: cover; border-radius: 0.25rem; box-shadow: 0 1px 4px rgba(0, 0, 0, 0.25); }
  .about-break { clear: both; margin: 2.5rem 0 0.5rem; border: 0; border-top: 1px solid rgba(128, 128, 128, 0.3); }
  .about-section { clear: both; padding-top: 2.25rem; }
  .about-section h2 { display: flex; align-items: center; gap: 0.5rem; font-size: 0.8rem; font-weight: 500; letter-spacing: 0.08em; text-transform: uppercase; margin: 0 0 0.9rem; color: var(--about-accent); }
  .about-section h2::before { content: ""; width: 0.2rem; height: 1rem; border-radius: 0.1rem; background: var(--about-accent); }
  .about-section strong { font-weight: 400; }
  .pub-list { margin: 0; padding-left: 1.25rem; }
  .pub-list li { padding-left: 0.25rem; margin-bottom: 0.9rem; }
  .pub-list li::marker { color: var(--about-accent); font-weight: 600; }
  .pub-title { font-size: 0.85rem; }
  .pub-authors { font-size: 0.75rem; opacity: 0.8; margin-top: 0.15rem; }
  .pub-authors strong { font-weight: 400; }
  .pub-tags { display: flex; flex-wrap: wrap; gap: 0.35rem; margin-top: 0.35rem; }
  .pub-tag { font-size: 0.7rem; line-height: 1; padding: 0.3rem 0.5rem; border: 1px solid var(--about-accent); border-radius: 0.25rem; color: inherit; text-decoration: none; }
  .pub-tag.primary { background: var(--about-accent); color: #fff; }
  [data-theme="dark"] .pub-tag.primary { color: #0b1220; }
  a.pub-tag:hover { background: var(--about-accent); color: #fff; }
  .news-list { list-style: none; margin: 0; padding: 0; }
  .news-list li { display: flex; gap: 1rem; margin-bottom: 0.5rem; }
  .news-date { flex: none; width: 5.5rem; font-size: 0.75rem; opacity: 0.65; padding-top: 0.1rem; }
  .about-section h2 a { color: inherit; text-decoration: none; }
  .post article .news { font-size: 0.85rem; }
  .post article .news table { margin: 0; }
  .post article .news th { width: 5.5rem !important; padding: 0 1rem 0.5rem 0; font-size: 0.75rem; font-weight: 300; opacity: 0.65; }
  .post article .news td { padding: 0 0 0.5rem; }
  .post article .news a { color: inherit; text-decoration: underline; text-decoration-color: rgba(128, 128, 128, 0.5); }
  @media (max-width: 575px) {
    .about-top { flex-direction: column; }
    .about-photo { order: -1; width: 220px; height: 220px; align-self: center; }
  }
  .timeline { list-style: none; margin: 0; padding: 0 0 0 1.25rem; border-left: 1px solid color-mix(in srgb, var(--about-accent) 40%, transparent); }
  .timeline li { position: relative; padding-bottom: 1rem; }
  .timeline li::before { content: ""; position: absolute; left: -1.5rem; top: 0.5rem; width: 0.5rem; height: 0.5rem; border-radius: 50%; background: var(--about-accent); }
  .timeline .date { font-size: 0.75rem; opacity: 0.65; }
  .timeline .title { font-size: 0.85rem; }
  .post article .social { text-align: left; margin: 0.75rem 0 0; }
  .post article .social .contact-icons { font-size: 1.5rem; }
  .post article .social .contact-icons a { margin: 0 0.8rem 0 0; }
</style>

<section class="about-section">
  <h2>News</h2>
  <ul class="news-list">
    {% for item in site.data.news %}
    <li><span class="news-date">{{ item.date }}</span><span>{{ item.text }}</span></li>
    {% endfor %}
  </ul>
</section>

<section class="about-section">
  <h2><a href="{{ '/blog/' | relative_url }}">Latest Posts</a></h2>
  {% include latest_posts.liquid %}
</section>

<section class="about-section">
  <h2>Publications</h2>
  <ol class="pub-list">
    <li>
      <div class="pub-title">비전 언어 모델(VLM) 기반 계층적 비디오 분석을 통한 멀티트랙 사운드 메타데이터 생성 파이프라인 개발</div>
      <div class="pub-authors"><strong>박광웅</strong>, 조영우, 차수환, 최재원, 장두성</div>
      <div class="pub-tags">
        <span class="pub-tag primary">KCC 2026</span>
        <span class="pub-tag">한국정보과학회 한국컴퓨터종합학술대회</span>
        <span class="pub-tag">pp. 2466–2468</span>
        <a class="pub-tag" href="https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12930030" target="_blank" rel="noopener">DBpia ↗</a>
      </div>
    </li>
  </ol>
</section>

<section class="about-section">
  <h2>Education</h2>
  <ul class="timeline">
    {% for item in site.data.education %}
    <li>
      <div class="date">{{ item.date }}</div>
      {% for line in item.items %}<div class="title">{{ line | markdownify | remove: '<p>' | remove: '</p>' }}</div>{% endfor %}
    </li>
    {% endfor %}
  </ul>
</section>

<section class="about-section">
  <h2>Awards</h2>
  <ul class="timeline">
    {% for item in site.data.awards %}
    <li>
      <div class="date">{{ item.date }}</div>
      {% for line in item.items %}<div class="title">{{ line | markdownify | remove: '<p>' | remove: '</p>' }}</div>{% endfor %}
    </li>
    {% endfor %}
  </ul>
</section>

<section class="about-section">
  <h2>Teaching &amp; Mentoring</h2>
  <ul class="timeline">
    {% for item in site.data.teaching %}
    <li>
      <div class="date">{{ item.date }}</div>
      {% for line in item.items %}<div class="title">{{ line | markdownify | remove: '<p>' | remove: '</p>' }}</div>{% endfor %}
    </li>
    {% endfor %}
  </ul>
</section>
