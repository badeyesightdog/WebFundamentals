---
layout: shared/narrow
title: "Architect the App Shell"
description: "What is an app shell and how do you architect a web app to use the app shell model?"
published_on: 2016-02-04
updated_on: 2016-02-04
translation_priority: 1
order: 2
authors:
  - petelepage
---

<p class="intro">
앱의 shell 은 점진적인 웹앱의 사용자 인터페이스를 움직이게 하는 데 요구되는 최소의 HTML, CSS, JavaScript이다. 신뢰할 만한 양질의 성능을 보장하는 요소 중의 하나이다. 그 것의 첫 로드는 반드시 굉장히 빨라야 하고 그 즉시 캐시되어야 한다. 이것은 shell 이 매 번 로드될 필요가 없고 대신에 필요한 내용만을 가져오는 것을 의미한다.
</p>

{% include shared/toc.liquid %}

앱셸 구조는 데이터로부터 주요 어플리케이션 기반과 UI를 구분한다. 모든 UI와 기반은 service worker를 사용하여 로컬 저장소에 캐시로 저장되는데 그 이후의 로드에선, 점진적인 웹앱이 모든 것을 로딩해야 하는 대신에 단지 필요한 데이터만을 가져올 수 있도록 하기 위함이다.

<figure>
  <img src="images/appshell.jpg" /> 
</figure>

바꿔 말하면,  앱셸은 당신이 앱스토어에 네이티브앱을 만들어 낼 때 퍼블리싱할 때의 코드 묶음과 비슷하다. 데이터를 포함하지는 않지만 당신의 앱이 잘 출발할 수 있게 하기 위해 필요한 주요 요소이다.

## 왜 앱셸 구조를 사용해야 하나?

앱셸 구조를 사용하면 점진적인 웹앱에 네이티브 앱과 비슷한 속성들을 제공하면서 속도에 집중할 수 있게 해준다: 즉각적인 로딩과 정기적인 갱신과 앱스토어 필요없이 모든 것을 말이다.

## 앱셸 디자인

첫 단계는 디자인을 그 주요 요소로 쪼개는 것이다.

스스로에게 물어보라:

* 화면에 바로 있어야 할 것은 무엇인가?
* 우리 앱에 핵심이 될 다른 UI 요소는 무엇인가? 
* 앱셸에 필요한 지원 자원은 무엇인가? 가령, 이미지, 자바스크립트, 스타일, 등등.

우린 첫 점진적인 웹앱으로 날씨앱을 만들 것이다. 핵심 요소는 다음으로 구성될 것이다:

<div class="mdl-grid">
  <div class="mdl-cell mdl-cell--6-col">
    <ul>
      <li>제목과 추가/갱신 버튼들로 구성된 머릿말</li>
      <li>예보 카드들을 담을 컨테이너</li>
      <li>예보 카드 템플릿</li>
      <li>새 도시를 추가할 대화창</li>
      <li>로딩 표시기</li> 
    </ul>
  </div>
  <div class="mdl-cell mdl-cell--6-col">
    <img src="images/weather-ss.png">
  </div>
</div>

더 복잡한 앱을 디자인할 때, 초기 로드에 필요하지 않은 내용이 나중에 요구되고 미래 사용을 위해 캐시될 수 있다. 가령,  첫 동작 경험을 렌더링한 이 후로 새 도시 대화창의 로딩을 연기하고 사용되지 않는 사용가능한 사이클을 가질 수 있다.
