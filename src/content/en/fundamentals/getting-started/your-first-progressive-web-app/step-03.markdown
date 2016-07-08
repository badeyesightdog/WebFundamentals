---
layout: shared/narrow
title: "Start with a Fast First Load"
description: "Fast first loads with Progressive Web Apps and the App Shell model."
published_on: 2016-02-04
updated_on: 2016-02-04
translation_priority: 1
order: 3
authors:
  - petelepage
notes:
  extra-credit: "For extra credit, replace the <code>localStorage</code> implementation with <a href='https://www.npmjs.com/package/idb'>idb</a>"
---

<p class="intro">
점진적인 웹앱은 빠르게 시작하고 즉시 사용가능해야 한다. 현재 상태에서 우리의 날씨앱은 빠르게 시작하지만 사용가능하진 않다. 데이터가 없다. 우린 데이터를 얻기 위해 AJAX 요청을 만들 수 있으나 그것은 추가 요청을 발생시키고 초기 로딩을 더 길게 만든다. 대신에, 첫 로딩에 실제 데이터를 제공하라.
</p>

{% include shared/toc.liquid %}

## 날씨 예보 데이터 주사

이 코드 실험을 위하여, 우린 정적으로 날씨 예보를 주사할 것이나 제품 단계 앱에서는 사용자 IP 주소의 지리적 위치에 근거한 서버에 의해 최신의 날씨 예보 데이터가 주사될 것이다.

즉시 호출되는 함수 표현의 안에 다음을 추가하라:

{% highlight javascript %}  
var initialWeatherForecast = {  
  key: 'newyork',  
  label: 'New York, NY',  
  currently: {  
    time: 1453489481,  
    summary: 'Clear',  
    icon: 'partly-cloudy-day',  
    temperature: 52.74,  
    apparentTemperature: 74.34,  
    precipProbability: 0.20,  
    humidity: 0.77,  
    windBearing: 125,  
    windSpeed: 1.52  
  },  
  daily: {  
    data: [  
      {icon: 'clear-day', temperatureMax: 55, temperatureMin: 34},  
      {icon: 'rain', temperatureMax: 55, temperatureMin: 34},  
      {icon: 'snow', temperatureMax: 55, temperatureMin: 34},  
      {icon: 'sleet', temperatureMax: 55, temperatureMin: 34},  
      {icon: 'fog', temperatureMax: 55, temperatureMin: 34},  
      {icon: 'wind', temperatureMax: 55, temperatureMin: 34},  
      {icon: 'partly-cloudy-day', temperatureMax: 55, temperatureMin: 34}  
    ]  
  }  
};
{% endhighlight %}

다음으로, 이제 더이상은 사용하지 않을 것이기 때문에 테스트를 위해 이전에 생성한 `fakeForecast` 데이터를 제거하라.

## 첫 구동 구별하기

그런데 우리가 이 정보(날씨앱이 캐시로부터 데이터를 가져올 때 미래 로딩에 적당하지 않을)를 표시할 때를 어떻게 알 수 있나? 사용자가 그 이후의 방문에 앱을 로드할 때, 그들이 도시를 변경했을 수도 있어서 우린 해당 도시들에 대한 정보를 불러올 필요가 있다; 단지 필연적으로 사용자가 전에 찾아본 본 첫 도시가 아니라 말이다.

사용자 선호, 사용자가 구독한 도시 목록과 같은 것은 IndexedDB 혹은 다른 빠른 저장방법을 사용하여 로컬에 저장되어야 한다. 이 예를 가능한 단순히 하기 위해, `localStorage` 를 사용했다; 제품 단계 앱에 대해선 이상적이지 않은 방법으로 그 이유는 이것이 차단적이고 동기적인 저장방법(어떤 기기에선 잠재적으로 매우 느린)이기 때문이다.

{% include shared/note.liquid list=page.notes.extra-credit %}

우선, 사용자 선로를 저장하기 위해 요구되는 코드를 `app.js` 내부에 추가해보자. :  

{% highlight javascript %}
// 로컬저장소에 도시 목록 저장, 로컬저장소에 대해 아래의 노트를 보자.
app.saveSelectedCities = function() {
  var selectedCities = JSON.stringify(app.selectedCities);
  // 중요: 로컬저장소의 사용에 대한 노트를 보라.
  localStorage.selectedCities = selectedCities;
};
{% endhighlight %}

다음엔, 사용자가 어떤 구독한 도시들이 있는 지 확인하고 있다면 렌더링하기 위한 시작 코드를 추가하고 아니라면 주사된 데이터를 사용하자. 다음의 코드를 당신의 `app.js`에 추가하라:  

{% highlight javascript %}
/****************************************************************************   
 *
 * 앱을 시작하기 위해 요구되는 코드
 *
 * 주의: 이 시작하기 안내를 간단히 하기 위해, 우린 로컬저장소를 사용했다.
 *   로컬저장소localStorage 는 동시발생의 API 이고 심각한 성능 영향을
 *   가지고 있다. 제품 앱의 경우엔 사용되면 안된다!
 *   대신에, IDB (https://www.npmjs.com/package/idb) 혹은
 *   SimpleDB (https://gist.github.com/inexorabletash/c8069c042b734519680c) 를 확인해보라.
 *
 ****************************************************************************/

app.selectedCities = localStorage.selectedCities;
if (app.selectedCities) {
  app.selectedCities = JSON.parse(app.selectedCities);
  app.selectedCities.forEach(function(city) {
    app.getForecast(city.key, city.label);
  });
} else {
  app.updateForecastCard(initialWeatherForecast);
  app.selectedCities = [
    {key: initialWeatherForecast.key, label: initialWeatherForecast.label}
  ];
  app.saveSelectedCities();
}
{% endhighlight %}

마지막으로, `app.saveSelectedCities();`를 `butAddCity` 이벤트핸들러에 추가함으로써 사용자가 새로운 도시를 추가할 때, 이 도시 목록을 저장하는 것을 잊지말라.

## 시험해보기

* 처음 구동할 때, 당신의 앱은 `initialWeatherForecast`로 부터 예보를 즉시 사용자에게 보여주어야 한다.
* 새로운 도시를 추가하고 두 개의 카드가 보이는 것을 확인하라.
* 브라우저를 새로고침하고 앱이 두 예보를 불러오고 최신 정보를 보여주는 것을 확인하라.

<a href="https://weather-pwa-sample.firebaseapp.com/step-05/" class="mdl-button mdl-js-button mdl-button--raised mdl-button--colored">시도하기</a>
