---
layout: shared/narrow
title: "Implement the App Shell"
description: "How do I use an an app shell within a Progressive Web App?"
published_on: 2016-02-04
updated_on: 2016-02-04
translation_priority: 1
order: 2
authors:
  - petelepage
notes:
  learn-about-wsk: "Learn more about the <a href='https://developers.google.com/web/tools/starter-kit/'>Web Starter Kit</a>"
  image-sprite: "Specifying each icon individually might seem less efficient compared to using an image sprite, but we'll cache those later as part of the app shell, ensuring that they're always available, without the need to make a network request."
  give-you: "We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code."
---

<p class="intro">
어떤 프로젝트와도 바로 시작할 수 있는 방법에는 여러가지가 있고 일반적으론 웹스타터킷 사용을 권장한다. 그러나 지금의 경우엔, 우리의 프로젝트를 가능한 단순하게 유지하고 점진적인 웹앱에 집중하기 위하여 필요한 모든 자원을 제공한다.
</p>

{% include shared/toc.liquid %}

## 코드 다운로드

쉽게 사용할 수 있도록 zip 파일로 [이 점진적인 웹앱 안내를 위한 모든 코드 다운로드](pwa-weather.zip) 할 수 있다. 개별 단계와 필요한 모든 자원은 zip 안에서 이용가능하다.

## 앱셸을 위한 HTML 생성

가능한 깨끗하게 시작하기 위해 완전히 새로운 `index.html` 파일로 시작할 것이다. 그리고 [Architect the App Shell](step-01)에서 논의된 주요 요소를 추가할 것이다.

기억하라, 핵심 요소들은 다음과 같이 구성될 것이다:

* 추가/갱신 버튼들과 제목이 있는 머릿말
* 예보 카드들을 담는 컨테이너
* 예보 카드 템플릿
* 새 도시 추가를 위한 대화창
* 로딩 표시기

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <!-- 이곳에 link to styles.css 삽입 -->
</head>
<body>
  <header class="header">
    <h1 class="header__title">Weather App</h1>
    <button id="butRefresh" class="headerButton"></button>
    <button id="butAdd" class="headerButton"></button>
  </header>

  <main class="main" hidden>
    <!-- 이곳에 forecast-card.html 삽입 -->
  </main>

  <div class="dialog-container">
    <!-- 이곳에 add-new-city-dialog.html 삽입 -->
  </div>

  <div class="loader">
    <svg viewBox="0 0 32 32" width="32" height="32">
      <circle id="spinner" cx="16" cy="16" r="14" fill="none"></circle>
    </svg>
  </div>

  <!-- 여기에 link to app.js 삽입 -->
</body>
</html>
{% endhighlight %}

기본적으로 `main` 내용이 숨겨지고`hidden` 로더는 볼 수 있음을 주의하라. 이것은 페이지가 로드되자마자 사용자가 로더를 볼 수 있게 하는 데, 이는 내용이 로딩되는 중이라는 것을 알리는 분명한 표시를 제공한다.

다음은, 예보 카드와 새 도시 추가 대화창을 추가해 보자. 시간을 절약하기 위해, 이것들은 `resources` 디렉토리에 제공되어서 상응하는 위치에 당신이 간단히 복사 후 붙여넣기할 수 있다.

## 주요 UI 요소에 스타일 적용

주요 스타일을 추가할 때다. 우리의 제작과 배치 과정중의 일부로서, 우린 문서 body 요소에 이 중심 스타일들을 inline으로 넣을 것이다. 그러나 지금은 분리된 CSS 파일에 넣어보자.

`index.html` 파일에, `<!-- Insert link to styles here -->` 의 부분을 다음으로 교체하라: 
{% highlight html %} 
<link rel="stylesheet" type="text/css" href="styles/inline.css">
{% endhighlight %}

시간 절약을 위해, 당신이 사용할 [stylesheet](https://weather-pwa-sample.firebaseapp.com/styles/inline.css)를 미리 생성했다. 시간을 할애해서 이를 보고 당신만의 것으로 만들기 위해 좀 손을 보자.

{% include shared/note.liquid list=page.notes.image-sprite %}

## 검사하고 조정하기

이제 검사할 시간이다, 어떻게 보이는지 보고 원하는 방식으로 조정해보자. `main` 컨테이너의 `hidden` 속성을 제거함으로써 예보 카드의 렌더링과 카드에 가짜 데이터를 추가해 봄으로써 확실히 검사하라.

{% include shared/remember.liquid list=page.notes.give-you %}

현재 이 앱은 무리없이 반응적이나 완벽하진 않다. 반응성을 향상시키고 다른 기기등에서도 정말 빛나게 만들 추가 스타일을 더해보자. 또한 좀 더 당신만의 것처럼 만들 수 있는 게 무언지 고려해 보라.

## 핵심 자바스크립트 부트스트랩 방식의 코드 추가

대부분의 UI가 준비되었으니, 모든 것이 동작하도록 만들기 위한 코드를 연결하기 시작할 때이다. 앱셸의 나머지와 같이, 핵심 경험의 일부로서 어떤 코드가 필요하고 어떤 게 나중에 로드되어야 할 지 알고 있어야 한다.

우리의 부트스트랩 코드에서 다음을 포함하였다:

* 앱에 필요한 어떤 핵심 정보를 포함하는 어떤 `app` 객체.
* 머릿말안(`add`/`refresh`)과 도시 추가 대화창안(`add`/`cancel`)의 모든 버튼에 관련된 이벤트리스너들.
* 예보 카드들을 추가하고 갱신하는 메소드 (`app.updateForecastCard`).
* Firebase Public Weather API에서 최신 날씨 예보 데이터를 가져오는 메소드 (`app.getForecast`).
* 현재 카드를 반복하고 최신 예보 데이터를 얻기 위해 `app.getForecast`를 호출하는 메소드 (`app.updateForecasts`).
* 어떻게 렌더링되는 지 빨리 검사하기 위해 사용할 수 있는 어떤 가짜 데이터 (`fakeForecast`).

자바스크립트 코드를 추가하라

1. `resources`디렉토리에서 `step3-app.js`를 복사해서 당신의 `scripts` 폴더로 옮기고 `app.js`라고 이름을 수정한다.
1. `index.html` 파일에서, 새로 생성된 `app.js`로 연결되는 링크를 추가하라.<br/>
   `<script src="/scripts/app.js"></script>`

## 시험해보기

주요 HTML, 스타일과 자바스크립트를 추가했으니, 이제 앱을 시험해 볼 시간이다. 아직 그리 많진 않을 테지만, 콘솔에 오류가 나오지 않도록 주의하라.

가짜 날씨 데이터가 어떻게 렌더링되는지 보기 위해, 당신의 `app.js` 파일 아래에 다음을 추가하라:  
`app.updateForecastCard(fakeForecast);`

<a href="https://weather-pwa-sample.firebaseapp.com/step-04/" class="mdl-button mdl-js-button mdl-button--raised mdl-button--colored">시도하기</a>
