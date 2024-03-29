# 43장 Ajax

## 43.1 Ajax란?

- Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.
- 이전의 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다. 따라서 화면이 전환되면 서버로부터 새로운 HTML 을 전송받아 웹페이지 전체를 처음부터 다시 렌더링했다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d55688d-c75a-4329-bb95-4389bd5ac208/Untitled.png)

- 이러한 전통적인 방식은 단점이있다.
  1. 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
  2. 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.
  3. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서보로부터 응답이 있을 때까진 다음 처리는 블로킹된다.
- 서버로부터 웹페이지의 변경에 피요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해졌다.
- 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 퍼포먼스와 부드러운 화면 전환이 가능해졌다.
- Ajax는 전통적인 방식과 비교했을 때 다음과 같은 장점이 있다.
  1. 변경할 부분을 갱신하는 데 핑요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
  2. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
  3. 클라이언트와 서보와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## 43.2 JSON

<aside>
📌 JSON은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

</aside>

### 43.2.1 JSON 표기 방식

- JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.

```jsx
{
	"name" : "Lee",
	"age" : 20,
	"alive" : true,
	"hobby" : ["traveling", "tennis"]
}
```

- JSON의 키는 반드시 큰따옴표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 문자열은 반드시 큰따옴표로 묶어야 한다.

### 43.2.2 JSON.stringify

- JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화라 한다.

### 43.2.3 JSON.parse

- JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화라 한다.

## 43.3 XMLHttpRequest

- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

### 43.3.1 XMLHttpRequest 객체 생성

- XMLHttpRequest \***\*객체는\*\*** XMLHttpRequest 생성자 함수를 호출하여 생성한다. XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.

```jsx
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();
```

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

- XMLHttpRequest 객체는 다양한 프로퍼티와 메서드를 제공한다. 대표적인 프로퍼티와 메서드는 다음과 같다.
  **XMLHttpRequest 객체의 프로토타입 프로퍼티**
  **XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티**
  **XMLHttpRequest 객체의 메서드**
  **XMLHttpRequest 객체의 정적 프로퍼티**

### 43.3.3 HTTP 요청 전송

- HTTP 요청을 전송하는 경우 다음 순서를 따른다.
  1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
  2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다
  3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send();
```

**XMLHttpRequest.prototype.open**

- open메서드는 서버에 전송할 HTTP요청을 초기화 함

```jsx
xhr.open(method, url[, async])
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f8772d02-01ee-4952-84b2-38da9d9a5177/Untitled.png)

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7ae5025-b090-42b0-807c-08343a31e387/Untitled.png)

### XMLHttpRequest.prototype.send

- send메서드는 open메서드로 초기화된 HTTP요청을 서버에 전송함
- GET요청일 경우, 데이터의 URL의 일부분인 쿼리 문자열로 서버에 전송함
- POST요청일 경우, 데이터를 request body에 담아 전송함

```jsx
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

- 요청 메서드가 GET인 경우, send메서드에 페이로드로 전달된 인수는 무시되고 요청 몸체는 null로 설정됨

### XMLHttpRequest.prototype.setRequestHeader

- setRequestHeader메서드는 특정 HTTP요청의 헤더 값을 설정함
- 반드시 open메서드를 호출한 이후에 호출해야 함
- Content-type은 request body에 담아 전송할 데이터의 MIME타입의 정보를 표현함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fdc98129-7cae-4917-992c-84aab3987c6f/Untitled.png)

```jsx
xhr.setRequestHeader("content-type", "application/json");

xhr.setRequestHeader("accept", "application/json");
```

- • Accept헤더를 설정하지 않으면 send메서드가 호출될 때 Accept헤더가 */*으로 전송된다.

### **43.3.4 HTTP 응답 처리**

- 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange이벤트를 캐치해여 HTTP 응답을 처리할 수 있다.
- XMLHttpRequest 객체는 브라우저에서 제공하는 WebAPI이므로 반드시 브라우저 환경에서 실행해야한다.
- HTTP 요청에 대한 응답이 정상적으로 도착했다면 요청에 대한 응답 몸체에서 서버가 전송한 데이터를 취득한다. 200 or error
