# PWA (Progressive Web Apps)

##### 출처 

```
https://www.samsungsds.com/global/ko/support/insights/pwa1.html
https://www.samsungsds.com/global/ko/support/insights/pwa2.html
https://codevkr.tistory.com/85

React로 프로그레시브 웹 앱(Progressive Web App) 만들기
https://chaewonkong.github.io/posts/pwa.html

프로그레시브 웹앱이란? 간단한 소개
https://www.youtube.com/watch?v=BZUEEby0HEU
프로그레시브웹앱 React 로 가볍게 시작하기
https://www.youtube.com/watch?v=rAx2x6CSnws

```

### PWA란?

---

- PWA는 Progressive Web App의 약자로, **웹**의 장점과 **앱**의 장접을 결합한 환경
- 웹 브라우저를 통해 앱 설치 없이 네이티브 앱의 기능을 사용할 수 있도록 하는 것이 목적



### 속성 혹은 특징

---

<img src="https://t1.daumcdn.net/cfile/tistory/997DE5465C35DEEA19">

#####   Alex Russell이 자신의 블로그에 게재한 PWA의 속성(attribute)은 다음과 같다.

- Responsive to fit any form factor
  \- 기기와 해상도에 상관없어야 함
- Connectivity independent Progressively-enhanced with Service Workers to let them work offline
  \- Service Worker가 오프라인에서도 동작할 수 있어야 함
- App-like-interactions Adopt a Shell + Content application model to create appy navigations & interactions
  \- 앱처럼 반응할 수 있을 것 + 속도와 모델을 정의
- Fresh Transparently always up-to-date thanks to the Service Worker update process
  \- 최신 콘텐츠를 유지할 수 있는 방법을 갖출 것
- Safe Served via TLS (a Service Worker requirement) to prevent snooping
  \- https에서 동작 가능할 것
- Discoverable Are identifiable as “applications” thanks to W3C Manifests and Service Worker registration scope allowing search engines to find them
  \- 매니페스트와 서비스 워커 등록 스코프를 검색엔진에서 찾을 수 있어야 함
- Re-engageable Can access the re-engagement UIs of the OS; e.g. Push Notifications
  \- 비활성화 상태에서도 푸시 알림을 받아 다시 실행시킬 수 있어야 함
- Linkable meaning they’re zero-friction, zero-install, and easy to share. The social power of URLs matters.
  \- URL로 공유 가능해야 함



### Service Worker

---

- offline / 느린 네트워크에서 작동하도록 함

- 브라우저가 백그라운드에서 실행되는 스크립트, 웹페이지와는 별개로 작동

- 웹페이지와 상호작용이 필요없는 푸시 알림, 백그라운드 동기화 등의 기능에 사용

- 프로그래밍이 가능한 네트워크 프록시(proxy)로, DOM을 직접 다룰 수는 없지만 postMessage를 통해서는 가능

- 브라우저가 닫혀도 백그라운드에서 돌아가지만, 프로세스를 종료시켜 정지 가능

- offline application을 위한 대안인 App Cache를 대체할 수 있음

  - <a href="https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers">서비스워커 사용해보기</a>

  - <a href="https://github.com/mozilla/serviceworker-cookbook/">서비스워커 cookbook</a>

  - <a href="https://github.com/mdn/sw-test">이해하기 쉬운 프로젝트</a>
  - <a href="https://keen.devpools.kr/sw-test/">따라해보기</a>

- 서비스워커와 웹 워커, 웹 소켓은 굉장히 비슷한 콘셉이면서 분명히 다르며, 차이점 설명
  - <a href="https://aarontgrogg.com/blog/2015/07/20/the-difference-between-service-workers-web-workers-and-websockets/">Service Worker vs Web Worker vs Web Socket</a>
- couchdb의 브라우저 버전
  - 서비스 워커를 이용한 오프라인 프로그램이 생겨남에 따라 애플리케이션 내부에 데이터 베이스를 만들고, 온라인이 될 때에 couchdb와 dbsync(데이터 베이스 동기화)를 하는 패턴의 개발이 증가 중
  - <a href="https://pouchdb.com/">pouchdb</a>

- 웹 브라우저를 통해 푸시 서비스 등록 (웹 앱에 푸시 서비스 추가)
  - <a href="https://pouchdb.com/">Push API on Web</a>
- 매니페스트를 통해 인스톨 여부의 생성 방법 확인 가능
  - <a href="https://developers.google.com/web/fundamentals/app-install-banners/">Manifest Web App install banner</a>



### App Shell Model

---

- ##### App Shell

  - 상단바, 메뉴, 배경이미지 등 매 로딩 시 바뀔 필요가 없는 것들
  - 브라우저가 캐싱해두면 오프라인 상태라 해도 앱 셸은 정상적으로 로딩 가능

- App Shell은 레이아웃 및 정적 리소스들을 모두 캐시화하며 다음과 같은 특징을 가지고 있음

  - 빠른 로드 / 최소 데이터 사용 / 로컬 캐시에서 정적 자산 사용
  - 내비게이션과 콘텐츠 분리 / 페이지별 콘텐츠(HTML, JSON 등) 검색 및 표시
  - 선택적으로 동적 컨텐츠 캐싱

- ##### Content

  - 로딩 시마다 바뀌는 정보 (글 목록, 내용, 이미지 등)



### 적용 예시

---

- https://pokedex.org/

- ##### 루리웹 App Manifest

  - 소스코드에 PWA가 적용된 부분

```
{
"short_name": "루리웹",
"name": "루리웹",
"icons": [
{
"src": "/assets/img/icon/ruliweb_icon_48_48.png",
"type": "image/png",
"sizes": "48x48"
},
{
"src": "/assets/img/icon/ruliweb_icon_96_96.png",
"type": "image/png",
"sizes": "96x96"
},
{
"src": "/assets/img/icon/ruliweb_icon_192_192.png",
"type": "image/png",
"sizes": "192x192"
}
],
"background_color": "#1A70DC", // 화면이 뜰 때
"theme_color": "#1A70DC", // 툴바
"display": "standalone", // 브라우저의 UI를 숨기고 띄울 때
"prefer_related_applications": true,
"orientation":"portrait", // portrait, landscape
"related_applications": [
{
"platform": "play",
"id": "com.ruliweb.www.ruliapp"
}
],
"start_url": "/"
}
```

