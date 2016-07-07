---
layout: shared/wide
description: "Web Fundamentals is a comprehensive resource for multi-device web development."
title: Web Fundamentals
translation_priority: 0
---

<div class="wf-subheading wf-fundamentals-landing">
  <div class="page-content">
    {% include svgs/fundamentals.svg %}
    <p class="mdl-typography--font-thin">

      Web<b>Fundamentals</b> 은 당신의 웹프로젝트에 알맞은 특징과 경험을 더하도록 돕기 위하여 고안된 웹 개발 업무처리 모범규준응 위한 포괄적인 자산이다. 만약 웹개발이 처음이거나 단지 당신의 프로젝트를 더 보기 좋게 하기 위함이라면, 그것을 충족시킬 것이다.(we’ve got you covered.)
    </p>
  </div>
</div>

{% include page-structure/site-promo-banner.liquid %}

<div class="page-content mdl-grid wf-fundamentals-cta">

  {% include shared/base_card.liquid text="<b>Progressive 진보적인/점진적인 웹앱</b>이 무엇이고 그것을 만들기 위해선 어떤 걸 알아야 할까? 이 단계별 안내서에서, 당신만의 진보적인/점진적인 웹앱을 만들고 진보적인/점진적인 웹앱을 만들기 위해 필요한 기초를 배울 것이다." linkHref="/web/fundamentals/getting-started/your-first-progressive-web-app/" linkText="시작하기" imgUrl="/web/fundamentals/imgs/vm-pwa.png" %}

  {% include shared/base_card.liquid text="사용자들이 속보및 새 글에 대한 정보에 관심을 갖도록 하기 위해 당신의 웹앱에 <b>알림 내보내기</b>를 더하는 법을 배워라." linkHref="/web/fundamentals/getting-started/push-notifications/" linkText="더 배우기" imgUrl="/web/fundamentals/imgs/notif-example.png" %}

</div>

<div class="wf-secondaryheading">
  <div class="page-content">
    <h3>준비, 설정, 코딩하기!</h3>
    <p>
      이미 맘 속에 생각해둔 것이 있는가? 그렇다면 바로 뛰어들라!
    </p>
    <div class="mdl-grid mdl-typography--text-center wf-fundamentals-areas">
      {% for pageInSection in page.context.subdirectories %}
      {% if pageInSection.index.published != false %}
      {% if pageInSection.id != 'getting-started' and pageInSection.id != 'primers' %}
      {% capture icon %}svgs/{{pageInSection.id}}.svg{% endcapture %}
        <div class="mdl-cell mdl-cell--4-col">
          <div class="icon">
            <a href="{{pageInSection.index.canonical_url }}">
              {% include {{icon}} %}
            </a>
          </div>
          <h3>
            <a href="{{pageInSection.index.canonical_url }}">
            {{pageInSection.index.title}}
            </a>
          </h3>
          <p>{{pageInSection.index.description}}</p>
        </div>
      {% endif %}
      {% endif %}
      {% endfor %}
    </div>
  </div>
</div>
