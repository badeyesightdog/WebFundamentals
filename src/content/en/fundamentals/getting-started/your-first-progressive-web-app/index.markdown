---
layout: shared/narrow
title: "Your First Progressive Web App"
description: "Progressive Web Apps are experiences that combine the best of the web and the best of apps. In this step-by-step guide, you'll build your own Progressive Web App and learn the the fundamentals needed for building Progressive Web Apps, including the app shell model, how to use service workers to cache the App Shell and your key application data and more."
published_on: 2016-02-04
updated_on: 2016-02-04
translation_priority: 1
order: 1
authors:
  - petelepage
notes:
  devsummit-video: "Looking for more? Watch Alex Russell talk about <a href='https://www.youtube.com/watch?v=MyQ8mtR9WxI'>Progressive Web Apps</a> from the 2015 Chrome Dev Summit"
---

<p class="intro">
<a href="/web/progressive-web-apps">점진적인 웹앱</a> 은 최상의 웹과 최고의 앱을 통합한 경험이다. 이것은 설치가 요구되지 않으며 브라우저 탭을 통하여 맨 처음 방문한 사용자에게 유용하다. 사용자가 시간이 지나며 앱과 점진적인 관계를 이뤄가기 때문에, 점점 더 강력해 진다. 더 쉽게 로드되며, 심지어 좋지 않은 네트워크상황에서도 그러하며, 알맞은 알림을 내보내고, 홈 화면에선 아이콘을 가지고 있고 최상단으로 로드한다.(It loads quickly, even on flaky networks, sends relevant push notifications, has an icon on the home screen and loads as a top-level, full screen experience.)
</p>

{% include shared/toc.liquid %}

## 점진적인 웹앱이란?

점진적인 웹앱은:

* **점진적인** - 점진적인 향상을 중점으로 하여 구성되기 때문에 브라우저 선택에 상관없이 모든 사용자에 동작한다.
* **반응적인** - 어떤 형태에도 맞는다: 데스크탑, 모바일, 테블릿, 혹은 그 다음에 올 어떤 기기에도 말이다.
* **독립적인 접속가능성** - 오프라인 혹은 저사양 네트워크에서도 작업이 가능한 service workers 로 향상.
* **앱과 같은** - app shell model 에서 구성되기 때문에, 앱 스타일의 반응과 네비게이션이 사용자에겐 앱처럼 느껴진다.
* **신선한** - service worker 업데이트 과정 덕분에 언제나 최신
* **안전한** - 권한없는 자의 접근을 막고 내용이 간섭받지 않도록 하기 위해 HTTPS 를 통하여 전달된다.
* **발견가능한** - W3C 적하목록과 검색엔진이 찾도록 돕는 service worker 등록범위 덕분에 응용프로그램으로서 동일함을 증명할 수 있다. 
* **다시 관여케하는** - 알림 내보내기와 같은 기능을 통하여 쉽게 다시 관여하게 만든다.
* **설치 가능한** - 앱스토어의 번거로움 없이 홈화면에 사용자가 가장 유용하다고 느끼는 앱들을 유지할 수 있게 한다.
* **연결가능한** - 복잡한 설치을 요구하지 않고 URL을 통해 쉽게 공유한다.

이 시작하기 안내는 당신만의 점진적인 웹앱 제작을 보여줄 것이다: 당신의 앱이 점진적인 웹앱의 주요 원칙을 충족하게 하는 실행 상세사항등과 더불어 디자인 고려등을 포함한다.

{% include shared/note.liquid list=page.notes.devsummit-video %}

## What are we going to be building? 무엇을 만들 것인가?

<div class="mdl-grid">
  <div class="mdl-cell mdl-cell--6-col">
    <p>
      시작하기 안내에선, 점진적인 웹앱 기술을 사용한 날씨웹앱을 만들 것이다.
    </p>
    <p>
      점진적인 웹앱의 속성들을 생각해 보자:
      <ul>
        <li><b>점진적인</b> - 전체적으로 점진적인 향상을 사용할 것이다.</li>
        <li><b>반응적인</b> - 어떤 기기 형태에서든지 맞도록 할 것이다.</li>
        <li><b>독립적인 접속가능성</b> - service workers로 app shell에 캐시 저장할 것이다.</li>
        <li><b>앱과 같은</b> - 도시를 추가하고 데이터를 새로고침할 때 앱 스타일의 반응을 사용할 것이다.</li>
        <li><b>신선한</b> - service workers로 가장 최신의 데이터를 캐시 저장할 것이다.</li>
        <li><b>안전한</b> - HTTPS를 지원하는 호스트에 앱을 배치할 것이다.</li>
        <li><b>발견가능하고 설치가능한</b> - 검색엔진이 우리의 앱을 쉽게 검색하도록 만드는 적하목록을 포함할 것이다.</li>
        <li><b>연결가능한</b> - 이건 웹이다!</li>
      </ul>
    </p>
  </div>
  <div class="mdl-cell mdl-cell--6-col">
    <a href="https://weather-pwa-sample.firebaseapp.com/final/">
      <img src="images/weather-ss.png">
    </a>
    <p>
      <a href="https://weather-pwa-sample.firebaseapp.com/final/" class="mdl-button mdl-js-button mdl-button--raised mdl-button--colored">Try it</a>
    </p>
  </div>
</div>

## 당신이 배울 것은

* "app shell" 방법을 사용하여 앱을 디자인하고 구성하는 법
* 오프라인에서도 앱이 동작하도록 만드는 법
* 추후 오프라인 사용을 위해 데이터를 저장하는 법

## 다뤄진 주제들

<ol>
{% for pageInSection in page.context.pages %}
  <li>
    <a href="{{pageInSection.relative_url }}">
      {{pageInSection.title}}
    </a>
  </li>
{% endfor %}
</ol>

## 필요로 할 것은

* Chrome 47 or above 크롬 버전47 이상
* HTML, CSS 와 JavaScript 지식

이 시작하기 안내는 점진적인 웹앱을 목적으로 한 것이다. 어떤 개념들은 외관을 속이거나 (가령, 스타일 혹은 관련없는 자바스크립트 등의 )코드 덩어리가 당신이 쉽게 복사 후 붙여넣기 할 수 있도록 제공될 것이다.
