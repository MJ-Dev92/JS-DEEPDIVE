# 40장 이벤트

## 40.1 이벤트 드리븐 프로그래밍

- 브라저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생 시킨다. 예를 들어 클릭, 키보드 입력, 마우스 이동 등이 일어나면 브라우저는 이를 감지형 특정한 타입의 이벤트를 발생시킨다.
- 이벤트가 발생했을 때 호출될 함수를 이벤트 핸들러라 하고, 이벤트가 발생했을 때 브라우저에게 **이벤트 핸들러**의 호출을 위임하는 것을 **이벤트 핸들러** 등록이라 한다.
- 클릭 이벤트가 발생하면 특정 함수를 호출하도록 브라우저에게 위임 할 수 있다. 즉, 함수를 언제 호출할지 알 수 없으므로 개발자가 명시적으로 함수를 호출하는 것이 아니라 브라우저에게 함수 호출을 위임하는 것이다.
- 이벤트와 그에 대응하는 함수를 통해 사용자와 애플리케이션은 상호작용을 할 수 있다.
- 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 **이벤트 드리븐 프로그래밍**이라 한다.

### 40.2 이벤트 타입

- 이벤트 타입은 이벤트의 종류를 나타내는 문자열이다. 이벤트 타입은 약 200여 가지가 있다. 사용 빈도가 높은 이벤트를 보자.

### 40.2.1 마우스 이벤트

| 이벤트 타입 | 이벤트 발생시점                                                |
| ----------- | -------------------------------------------------------------- |
| click       | 마우스 버튼을 클릭했을 때                                      |
| dbclick     | 마우스 버튼을 더블 클릭했을 때                                 |
| mousedown   | 마우스 버튼을 눌렀을 때                                        |
| mouseup     | 누르고 있던 마우스 버튼을 놓았을 때                            |
| mousemove   | 마우스 커서를 움직였을 때                                      |
| moisenter   | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링 되지 않는다) |
| mouseover   | 마우스 커서를 HTML 요소안으로 이동했을 때 (버블링 된다.)       |
| mouseleave  | 마우스 커서를 HTML 요소 밖으로 이동했을 때                     |
| mouseout    | 마우스 커서를 HTML 요소 밖으로 이동했을 때                     |

## 40.3 이벤트 핸들러 등록

- 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록이라 한다.
- 이벤트 핸들러 등록하는 방법은 3가지다.

### 40.3.1 이벤트 핸들러 어트리뷰트 방식

- 이벤트 핸들러 어트리뷰트의 이름은 on 접수사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다.
- 주의할 점은 이벤트 핸들러 어트리뷰트 값으로 함수 참조가 아닌 함수 호출문 등의 문을 할당한다는 것이다.
- 만약 함수 참조가 아니라 함수 호출문을 등록하면 함수 호출문의 평가 결과가 이벤트 핸들러로 등록된다.
- 40-2 예제에서는 이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당했다. 이때 **이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다.**

### 40.3.2 이벤트 핸들러 프로퍼티 방식

- 이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체인 이벤트 타깃과 이벤트의 종류를 나타내는 문자열인 이벤트 타입 그리고 이벤트 핸들러를 지정할 필요가 있다.
- 예를 들어 버튼 요소가 클릭되면 handleClick 함수를 호출하도록 이벤트 핸들러를 등록하는 경우 이벤트 타깃은 버튼 요소이고 이벤트 타입은 click 이며 이벤트 핸들러는 handleClick 함수다.

### 40.3.3 addEventListener 메서드 방식

- addEventListener 메서드의 첫번쨰 매개변수에는 이벤트의 종류를 나타내는 문자열인 이벤트 타입을 전달한다.
- 두번 쨰 매개변수에는 이벤트 핸들러를 전달한다. 마지막 매개변수에는 이벤트를 캐치할 이벤트 전파단계를 지정한다.
- 생략하거나 false를 지정하면 버블링 단계에서 이벤트를 캐치하고, true를 지정하면 캡처링 단계에서 이벤트를 캐치한다.

## 40.4 이벤트 핸들러 제거

- addEventListener 메서드로 등록한 이벤트 핸들러를 제거하려면 EventTarget.prototype.removeEventListener 메서드를 사용한다. removeEventListener메서드에 전달한 인수는 addEventListener 메서드와 동일하다.
- 단, addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.

## 40.5 이벤트 객체

- 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다. **생성된 이벤트 객체는 이벤트 핸들러의 첫 번쨰 인수로 전달된다.**
- 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 e에 암묵적으로 할당된다.
- 이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할 때 이벤트 객체를 전달받을 매개변수를 명시적으로 선언해야 한다.
- 이벤트 핸들러 어트리뷰트 방식의 경우 이벤트 객체를 전달받으려면 이벤트 핸들러의 첫 번째 배개변수 이름이 반드시 event이어야 한다. 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성되는 이벤트 핸들러의 함수 몸체를 의미하기 떄문이다.

### 40.5.1 이벤트 객체의 상속 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/115f904c-c90b-4158-b0b0-f07037101660/Untitled.png)

- 이처럼 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성된다. 그리고 생성된 이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원이 된다.

### 40.5.2 이벤트 객체의 공통 프로퍼티

- Evaent 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다 이벤트 객체의 공통 프로퍼티는 다음과 같다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7f8c1f8-d790-46e9-9cb7-e44cc13a49ac/Untitled.png)

## 40.5.3 마우스 정보 취득

- click, dbclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성 되는 mouseEvent 타입의 이벤트 객체는 다음과 같은 고유의 프로퍼티를 갖닌다.
  - 마우스 포인터의 좌표 정보를 나타내는 프로퍼티 : screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY
  - 버튼 정보를 나타내는 프로퍼티 : altKey, ctrlKey, shiftKey, button
- 마우스 포인터 좌표는 MouseEvent 타입의 이벤트 객체에서 제공한다. MouseEvent 타입의 이벤트 객체는 마우스 포인터의 좌표 정보를 나타내는screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY 프로퍼티를 제공한다. 이 중에서 clientX/clientY는 뷰포트 즉 웹페이지의 가시 영역을 기준으로 마우스 포인터 좌표를 나타낸다.

### 40.5.4 키보드 정보 취득

- keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 altKey, CtrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로퍼티를 갖는다.

## 40.6 이벤트 전파

- DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트 DOM 트리를 통해 전파된다. 이를 이벤트 전파라고 한다.
- ul 요소의 두 번째 자식 요소인 li 요소를 클릭하면 클릭 이벤트가 발생한다. 이때 **생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중슴으로 DOM 트리를 통해 전파된다.**
- 이벤트 전파는 이벤트 객체가 전파되는 방향에 따라 다음과 같이 3단계로 구분할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3b9402b-de79-4ac8-a624-7e5f0afdf2cd/Untitled.png)

- **캡처링 단계** : 이벤트가 상위 요소에서 하위 요소 방향으로 전파
- **타킷 단계** : 이벤트가 이벤트 타깃에 도달
- **버블링 단계** : 이벤트가 하위 요소에서 상위 요소 방향으로 전파
- **이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있다.** 즉, DOM 트리를 통해 전파되는 이벤트는 이벤트 패스에 위치한 모든 DOM 요소에 캐치할 수 있다.

## 40.7 이벤트 위임

- 이벤트 위임은 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다.
- 이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다. 또한 동적으로 하위 DOM 요소를 추가하더라도 일일이 추가된 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.

## 40.8 DOM 요소의 기본 동작 조작

### 40.8.1 DOM 요소의 기본 동작 중단

- DOM 요소는 저마다 기본 동작이 있다. 예를 들어, a 요소를 클릭하면 href 어티리뷰트에 지정된 링크로 이동하고, checkbox 또는 radio 요소를 클랙히면 체크 또는 해제된다.
- 이벤트 객체의 preventDefault 메서드는 이러한 DOM 요소의 기본 동작을 중단시킨다.

### 40.8.2 이벤트 전파 방지

- 이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지시킨다.
- stopPropagation 메서드는 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트의 전파를 중단 시킨다.

## 40.9 이벤트 핸들러 내부의 this

### 40.9.1 이벤트 핸들러 어트리뷰트 방식

- handleClick 함수 내부의 this는 전역 객체 window를 가리킨다.
- 일반 함수로서 호출되는 함수 내부의 this는 전역 객체를 가리킨다. 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
- 이벤트 핸들러 어트리뷰트 방식에 의해 암묵적으로 생성된 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다. 이는 이벤트 핸들러 프로퍼티 방식과 동일하다.

### 40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

- 이벤트 핸등ㄹ러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 같다.
- 화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킨다. 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
- 클래스에서 이벤트핸들러를 바인딩 하는 경우 this를 주의해야한다. this는 클래스가 생성할 인스턴스를 가리키지 않는다. 메서드를 이벤트 핸들러로 바인딩할 때 bind 메서드를 사용해 this를 전달하여 클래스가 생성할 인스턴슬르 가리키도록 해야한다.

## 40.10 이벤트 핸들러에 인수 전달

- 함수에 인수를 전달하려면 함수를 호출할 떄 전달해야한다. 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 함수 호출문이 아닌 함수 자체를 등록해야 한다.

## 40.11 커스텀 이벤트

### 40.11.1 커스텀 이벤트 생성

- Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다. 이처럼 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라 한다.
- 이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 받는다. 일반적으로 CustomEvent 이벤트 생성자 함수를 사용한다.

## 40.11.2 커스텀 이벤트 디스패치

- 생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치 할 수 있다.
- dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생한다.
- 일반적으로 이벤트 핸들러는 비동기처리 방색으로 동작하지만 dispatchEvent 메서드는 이벤트 핸들러를 동기 처리 방색으로 호출한다
- 기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 한다. ‘on + 이벤트 타입’으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티 요소 노드에 존재하지 않기 때문이다.