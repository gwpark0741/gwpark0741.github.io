---
layout: page
permalink: /cv/
title: CV
nav: true
nav_order: 3
description:
---

<!-- CV PDF 는 _data/cv.yml 을 고쳐 main 에 올리면 .github/workflows/render-cv.yml 이 다시 만듭니다. -->

{% assign cv_pdf = '/assets/rendercv/rendercv_output/Gwangwoong_Park_CV.pdf' | relative_url %}

<p><a href="{{ cv_pdf }}" target="_blank" rel="noopener"><i class="fa-solid fa-file-pdf"></i> Download PDF</a></p>

<object data="{{ cv_pdf }}" type="application/pdf" width="100%" style="height: 85vh; border: 1px solid rgba(128, 128, 128, 0.3); border-radius: 0.25rem;">
  <p>Your browser cannot show the PDF here. <a href="{{ cv_pdf }}">Open the CV</a>.</p>
</object>
