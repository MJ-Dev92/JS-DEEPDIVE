# 44장 REST API

<aside>
📌 REST의 기본 원칙을 성실히 지킨 서비스 디자인을 RESTful이라고 표현한다. 즉, REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

</aside>

## 44.1 REST API의 구성

- REST API는 자원, 행위, 표현의 3가지 요소로 구성된다. REST는 자체 표현 구조로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

| 구성 요소 | 내용                           | 표현 방법       |
| --------- | ------------------------------ | --------------- |
| 자원      | 자원                           | UR(엔드포인트)  |
| 행위      | 자원에 대한 행위               | HTTP요청 메소드 |
| 표현      | 자원에 대한 행위의 구체적 내용 | 페이로드        |

## 44.2 REST API 설계 원칙

- REST 에서 가장 중요한 기본적인 원칙은 두 가지다. URI는 리소스를 표현하는 데 집중하고 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것이 RESTful API를 설계하는 중심 규칙이다.

### 1. URI는 리소스를 표현해야 한다.

- URI는 리소스를 표현하는 데 중접을 두어야 한다. 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다. 따라서 이름에 get 같은 행위에 대한 표현이 들어가서는 안 된다.

```jsx
// bad
GET / getTOdos / 1;
GET / todos / show / 1;

// good
GET / todos / 1;
```

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다. 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE)를 사용하여 CRUD를 구현한다.

## 44.3 JSON Server를 이용한 REST API 실습

### 44.3.1 JSON Server 설치

- JSON Server는 json 파일을 사용하여 가상 REST API 서버를 구출할 수 있는 툴이다. 먼저 npm을 사용하여 JSON Server를 설치하자

```jsx
$ npm inint -y
$ npm install json-server--save-dev
```

### 44.3.2 db.json 파일 생성

- 프로젝트 루트 폴더에 db.json 파일을 생성, db.json파일은 리소스를 제공하는 데이터베이스 역할을 한다.

### 44.3.3 JSON Server 실행

- 터미널에서 다음과 같이 명령어를 입력하여 JSON Server를 실행한다.

```jsx
// 기본 포트(3000) 사용 / watch 옵션 적용
$ json-server--watch db.json

// 명령어를 입력하여 JSON Server를 실행
$ npm start
```

### 44.3.4 GET 요청

### 44.3.5 POST 요청

- POST 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME타입을 지정해야 한다.

### 44.3.6 PUT 요청

- PUT은 특정 리소스 전체를 교체할 때 사용한다.
- PUT요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.

### 44.3.7 PATCH 요청

- PATCH는 특정 리소스의 일부를 수정할 떄 사용한다.
- PATCH요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.

### 44.3.8 DELETE 요청
