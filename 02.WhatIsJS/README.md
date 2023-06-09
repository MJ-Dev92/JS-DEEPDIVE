# 2장 자바스크립트란?

### 2-1 자바스크립트의 탄생

- 1995년 웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기로 결정하면서 탄생한 것이 브랜던 아이크가 개발한 자바스크립트다.
- 자바스크립트는 현재 모든 브라우저의 표준 프로그래밍 언어로 자리 잡았다. 그러나 자바스크립트가 순탄하게 성장했던 것은 아니다. 자바스크립트가 탄생한 뒤 얼마 지나지 않아 자바스크립트의 패상번전인 JScript가 출시되어 자바스크립트는 위기를 맞는다.

### 2-2 자바스크립트의 표준화

- 넷스케이프 커뮤니케이션즈와 마이크로소프트는 자사 브라우저의 시장 점유율을 높이기 위해 자사 브라우저에서만 동작하는 기능을 경쟁적으로 추가하기 시작했다.
- 이로 인해 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 크로스 브라우징 이슈가 발생하기 시작했고, 결과적으로 모든 브라우저에서 정상적으로 동작하는 웹페이지를 개발하기가 무척 어려워졌다.
- 자바스크립트의 파편화를 방지하고 모든 브라우저에서 정상적으로 동작하느 표준화된 자바스크립트의 필요성이 대두되면서 이를 위해 넷스케이프 커뮤니케이션즈는 컴퓨텨 시스템의 표준을 관리하는 비영리 표준화 기구인 ECMA 인터내셔널에 자바스크립트의 표준화를 요청한다.
- 1997년 7월 표준화된 자바스크립트 초판(ECMAScript 1) 사양이 완성되었고, 상표권 문제로 자바스크립트는 ECMAScript로 명명되었다. 이후 1999년 ECMAScript 3(ES3)이 공개되고 10년 만인 2009년에 ES5는 HTML5와 함께 출현한 표준사양이다.
- ECMAScript6(ECMAScript 2015, ES6)는 let/const 키워드, 화살표 함수, 클래스, 모듈 등과 같이 범용 프로그래밍 언어로서 갖춰야할 기능들을 대거 도입하는 큰 변화가 있었다.

### 2-3 자바스크립트 성장의 역사

Ajax

- 1999년 자바스크립트를 이용해 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신 기능인 Ajax가 XMLHttpRequest라는 이름으로 등장했다.
- 이전의 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML 코드를 서버로부터 전송받아 웹페이지 전체를 렌더링하는 방식으로 동작했다. 따라서 화면이 전환되면 서버로부터 새로운 HTML을 전송 받아 웹페이지 전체를 처음부터 다시 렌더링 했다.
- Ajax의 등장으로 웹페이지에서 변경할 필요가 없는 부분은 다시 렌더링하지 않고 서버로부터 필요한 데이터만 전송받아 변경해야 하는 부분만 한정적으로 렌더링하는 방식이 가능해 졌다.
- 2005년 구글이 발표한 구글 맵스 웹브라우저에서 자바스크립트와 Ajax를 기반으로 동작하는 구글 맵스가 데스크톱 애플리케이션과 비교했을 때 손색이 없을 정도의 성능과 부드러운 화면 전환 효과를 보여줬다.

jQuery

- 2006년 jQuery의 등장으로 다소 번거롭고 논란이 있던 DOM을 더욱 쉽게 제어할 수 있게 되었고 크로스 브라우징 이슈도 어느 정도 해결되었다. jQuery는 넓은 사용자 층을 순식간에 확보했다.

V8 자바스크립트 엔진

- 2008년 등장한 구글의 V8자바스크립트 엔진은 빠른 성능을 보여주었다. V8 자바스크립트 엔진 등장으로 사용자 경험(UX: user Experience)을 제공할 수 있는 웹 애플리케이션 프로그래밍 언어로 정착하게 되었다.
- V8 자바스크립트 엔진으로 촉발된 자바스크립트의 벌전으로 과거 웹 서버에서 수행되던 로직들이 더거 클라이언트로 이동했고, 이는 웹 애플리케이션 개발에서 프런트엔드 영역이 주목받는 계기로 작용했다.

Node.js

- 2009년 라이언 달이 발표한 Node.js는 구글 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경이다.
- 자바스크립트 엔진을 브라우저에서 독립시킨 자바스크립트 실행 환경이다.
- Node.js는 다양한 플랫폼에 적용할 수 있지만 서버 사이드 애플리케이션 개발에 주로 사용되며, 이에 필요한 모듈, 파일 시스템, HTTP 등 빌트인 API를 제공한다.
- Node.js는 비동기 I/O를 지원하며 단일 스레드 이벤트 루프 기반으로 동작함으로써 요청 처리 성능이 좋다. 따라서 Node.js는 데이터를 실시간으로 처리하기위해 I/O가 빈번하게 발생하는 SPA(Single Page Application)에 적합하다.

SPA 프레임워크

- 이전의 개발 방식으로는 복잡해진 개발 과정을 수행하기 어려워졌고, 이러한 필요에 따라 많은 패턴과 라이브러리가 출현했다.
- 그 덕분에 개발에 많은 도움을 주었지만 변경에 유연하면서 확장하기 쉬운 애플리케이션 아키텍처의 구축을 어렵게 했고, 필연적으로 프레임워크가 등장하게 되었다
- CBD(component based development)방번론을 기반으로 하는 SPA가 대장화 되면서 Angular, React, Vue.js, Svelte등 다양한 SPA 프레임워크/라이브러리 또한 많은 사용층을 확보하고 있다.

### 2-4 자바스크립트와 ECMAScript

- 자바스크립트는 일반적으로 프로그래밍 언어로서 기본 뼈대를 이루는 ECMAScript와 브라우저가 별도 지원하는 클라이언트 사이드 Web API즉 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Stroage, Web Component, Web Worker등을 아우르는 개념이다.

### 2-5 자바스크립트의 특징

<aside>
📌 자바스크립트는 HTML, CSS와 함께 웹을 구성하는 요소 중 하나로 웹 브라우저에서 동작하는 유일한 프로그래밍 언어다.

</aside>

- 자바스크립트는 개발자가 별도의 컴파일 작업을 수행하지 않는 **인터프리터 언어**다.
- 자바스크립트 엔진은 인터프리터와 컴파일러의 장점을 결합해 비교적 처리 속도가 느린 인터프리터의 단점을 해결했다.
- 인터프리터는 소스코드를 즉시 실행하고 컴파일러는 빠르게 동작하는 머신 코드를 생성하고 최적화한다.
- 인터프리터 언어 vs 컴파일러 언어 차이점 14p 참조
- 자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 **멀티** **패러다임 프로그래밍 언어**다.
- 자바스크립트는 클래스 기반 객체지향 언어보다 효율적이면서 강력한 **프로토타입 기반의 객체지향 언어**다.

### 2-6 ES6 브라우저 지원 현황

- 인터넷 익스플로러나 구형 브라우저는 ES6를 대부분 지원하지 않아 고려해야 하는 상황이라면 바벨과 같은 트랜스 파일러를 사용해 ES6 이상의 사양으로 구현한 소스코드를 ES5 이하의 사양으로 다운그레이드할 필요가 있다.
