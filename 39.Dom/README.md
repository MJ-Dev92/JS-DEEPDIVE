# 39장 DOM

<aside>
📌 DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

</aside>

## 39.1 노드

### 39.1.1 HTML 요소와 노드 객체

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/45d6250a-8348-4567-a4ba-d2b33a349d97/Untitled.png)

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c50fbc08-8c54-4b09-864b-b7b13c2a311f/Untitled.png)

- HTML 요소의 콘텐츠 영역에는 텍스트뿐만 아니라 다른 HTML 요소도 포함할 수 있다.
- 이때 HTML 요소 간에는 중첩 관계에 의해 계층적인 부자 관계가 형성된다. 이러한 HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성한다.

**트리 자료구조**

- 트리 자료구조는 노드들의 계층 구조로 이뤄진다. 트리 자료구조는 하나의 최상위 노드에서 시작한다. 최상위 노드는 부모 노드가 없으며, 루트 노드라 한다. 루트노드는 0개 이상의 자식노드를 갖는다. 자식 노드가 없는 노드를 리프 노드라 한다.
- **노드 객체들로 구성된 트리 자료구조를 DOM이라 한다**. 노드 객체의 트리로 구조화되어 있기 때문에 DOM을 **DOM 트리**라고 부르기도 한다.

### 39.1.2 노드 객체의 타입

- 렌더링 엔진은 HTML 문서를 파싱하여 다음과 같이 DOM을 생성한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be4982d5-574e-4639-a659-e44a4badcd2f/Untitled.png)

- DOM은 노드 객체의 계층적인 구조로 구성된다. 노드 객체는 종류가 있고 상속 구조를 갖는다. 노드 객체는 총 12개의 종류가 있다. 이 중에서 중요한 노드 타입은 다음과 같이 4가지다.

**문서 노드**

- 문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킨다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.
- 문서노드, 즉 document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당한다. 즉, 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.

**요소노드**

- 요소 노드는 HTML 요소를 가리키는 객체다. 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화 한다. 따라서 요소 노드는 문서의 구조를 표현한다고 할 수 있다.

**어트리뷰트 노드**

- 어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다. 어티르뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 단 요소 노드는 부모 노드와 연결되어 있지만 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.
- 즉, 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다.

**텍스트 노드**

- 텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다. 요소 노드가 문서의 구조를 표현한다면 텍스트 노드는 문서의 정보를 표현한다고 할 수 있다. 텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드다.
- 즉 텍스트노드는 DOM 트리의 최종단이다.

### 39.1.3 노드 객체의 상속 구조

- DOM은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다. 즉 DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다.
- DOM을 구성하는 노드 객체는 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체다. 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e9ba783c-cb6d-4d8b-8703-b628f6580f11/Untitled.png)

- 배열이 객체인 동시에 배열인 것처럼 input 요소 노드 객체도 다음과 같이 다양한 특성을 갖는 객체이며, 이러한 특성을 나타내는 기능들을 상속을 통해 제공받는다.

| input 요소 노드 객체의 특성                                | 프로토타입을 제공하는 객체 |
| ---------------------------------------------------------- | -------------------------- |
| 객체                                                       | Object                     |
| 이벤트를 발생시키는 객체                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체            | HTMLElement                |
| HTML 요소중에서 input 요소를 표현하는 객체                 | HTMLInputElement           |

- 노드 객체에는 노드 객체의 종류, 즉 노드타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고, 노드 타입에 따라 고유한 기능도 있다.
- 노드 객체는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적인 고유 기능일수록 프로토타입체인의 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.
- **DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.**
- 중요한 것은 DOM API, 즉 DOM이 제공하는 프로퍼티와 메서드를 사용하여 노드에 접근하고 HTML의 구조나 내용 또는 스타일 등을 동적으로 변경하는 방법을 익히는 것이다.
- 프론트엔드 개발자에거 HTML은 단순히 태그와 어트리뷰트를 선언적으로 배치하여 뷰를 구성하는 것 이상의 의미를 갖는다. 즉, HTML DOM과 연관 지어 바라보아야 한다.

## 39.2 요소 노드 취득

- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 한다.
- 요소 노드의 취득은 HTML 요소를 조작하는 시작점이다. 이를 위해 DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.

### 39.2.1 id를 이용한 요소 노드 취득

- Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다. getElementById 메서드는 Document.prototype의 프로퍼티다. 따라서 반드시 문서 노드인 document를 통해 호출해야 한다.

### 39.2.2 태그 이름을 이용한 요소 노드 취득

- Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다. 메서드 이름에 포함된 Elements가 복수형인 것에서 알 수 있듯이 여러개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.
- getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블 이다.

### 39.2.3 class를 이용한 요소 노드 취득

- Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다.

### 39.2.4 CSS 선택자를 이용한 요소 노드 취득

- CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.
- Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
  - 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환한다.
  - 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null을 반환한다.
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.
- querySelectorAll 메서드는 여러개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. NodeList 객체는 유사 배열 객체이면서 이터러블이다.
  - 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체를 반환한다.
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.
- id 어트리뷰트가 있는 요소 노드를 취득하는 경우에는 getElementById 메서드를 사용하고 그 외의 경우에는 querySelector, querySelectorAll 메서드를 사용하는 것을 권장한다.

### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인

- Element.prototype.matches 메서드는 인수로 전달한 CSS선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.

### 39.2.6 HTMLCollection과 NodeList

- HTMLCollection과 NodeList의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 **살아 있는 객체**라는 것이다. HTMLCollection은 언제나 live 객체로 동작한다.
- 단, NodeList는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있다.

**HTMLCollection**

- getElementsByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체다 따라서 HTMLCollection 객체를 살아 있는 객체라고 부르기도 한다.

**NodeList**

- HTMLCollection 객체의 부작용을 해결하기 위해 getElementsByTagName, getElementByClassName 메서드 대신 querySelectorAll 메서드를 사용하는 방법도 있다. querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. 이때 NodeList 객체는 실기산으로 노드 객체의 상태 변경을 반영하지 않는 객체다

```jsx
// querySelectorAll은 DOM 컬렉션 객체인 NodeList를 반환한다.
const $elems = document.querySelectorAll(".red");

// NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
$elems.forEach((elem) => (elem.clasName = "blue"));
```

- NodeList 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작한다. 하지만 **childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.**
- **따라서 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다.**

## 39.3 노드 탐색

- 요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드등을 탐색 해야 할 때가 있다.

```jsx
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="banana">Banana</li>
  <li class="orange">Orange</li>
</ul>
```

- 이처럼 DOM 트리 상위 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.
- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단, 노드 탐색 프로퍼티는 setter없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티다.

### 39.3.1 공백 텍스트 노드

- 지금까지 언급하지 않았지만 HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다. 이를 공백 텍스트 노드라 한다.
- 텍스트 에디터에서 HTML 문서에 스페이스 키, 탭 키, 엔터 키 등을 입력하면 공백 문자가 추가된다.
- 이처럼 HTML 문서의 공백 문자는 공백 텍스트 노드를 생성한다. 따라서 노드를 탐색할 떄는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 한다. 만약 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않지만 가독성이 좋지 않으므로 권장하지 않는다.

### 39.3.2 자식 노드 탐색

- 자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.

| 프로퍼티                            | 설명                                                                                                                                                                      |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.childNode            | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환한다. childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다  |
| Element.prototype.children          | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환한다. children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않는다. |
| Node.prototype.firstChild           | 첫 번째 자식 노드를 반환한다. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.                                                                          |
| Node.prototype.lastChild            | 마지막 자식 노드를 반환한다. lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.                                                                            |
| Node.prototype.firstElementChild    | 첫 번째 자식 요소 노드를 반환한다. firstElementChild 프로퍼티는 요소 노드만 반환한다.                                                                                     |
| Node.prototype.lastElementlastChild | 마지막 자식 요소 노드를 반환한다. lastElementChild 프로퍼티는 요소 노드만 반환한다.                                                                                       |

### 39.3.3 자식 노드 존재 확인

- 자식 노드가 존재하는지 확인하려면 Node.prototype.hasChildNodes 메서드를 사용한다. 자식 노드가 존재하면 true, 존재하지 않다면 false를 반환한다.

### 39.3.4 요소 노드의 텍스트 노드탐색

- 부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다. 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 겨우는 없다.

## 39.4 노드 정보 취득

| 프로퍼티                | 설명                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------- |
| Node.prototype.nodeType | 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다. 노드 타입 상수는 Node에 정의되어 있다. |

- Node.ELEMENT_NODE: 요소 노드타입을 나타내는 상수 1을 반환
- Node.TEXT_NODE: 텍스트 노드 타입을 나타나는 상수 3을 반환
- Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환 |
  | Node.prototype.nodeName | 노드 이름을 문자열로 반환한다.
- 요소 노드 : 대문자 문자열로 태그이름을 반환
- 텍스트 노드 : 문자열 “#text”를 반환
- 문서 노드 : 문자열 “#document”를 반환 |

## 39.5 요소 노드의 텍스트 조작

### 39.5.1 nodeValue

- 지금까지 살펴본 노드 탐색, 노드 정보 프로퍼티는 모두 읽기 전용 접근자 프로퍼티다. 지금부터 살펴볼 Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. 따라서 nodeValue 프로퍼티는 참조와 할당 모두 가능하다.
- 텍스트 노드가 아닌 노드를 nodeValue 프로퍼티를 참조하면 null을 반환한다.
- 요소 노드의 텍스트를 변경하려면 다음과 같은 순서의 처리가 필요하다.
  1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노도의 텍스트 노드를 탐색한다. 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색한다.
  2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.

### 39.5.2 textContent

- Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다. HTML 마크업은 무시된다.
- 참고로 textContent 프로퍼티와 유사한 동작을 하는 innerText 프로퍼티가 있다. innerText프로퍼티는 다음과 같은 이유로 사용하지 않는 것이 좋다.
  - innerText 프로퍼티는 CSS에 순종적이다. 예를 들어 innerText 프로퍼티는 CSS에 의해 비표시로 지정된 요소 노드의 텍스트를 반환하지 않는다.
  - innerText프로퍼티는 CSS를 고려해야하므로 textContent 프로퍼티보다 느리다.

## 39.6 DOM 조작

- DOM조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.
- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다. 따라서 복잡한 콘텐츠를 다루는 DOM 조작은 성능 최적하를 위해 주의해서 다루어야 한다.

### 39.6.1 innerHTML

- Element.prototype.innerHTML 프로퍼티 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경한다. 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.
- 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하므로 위험하다.

### 39.6.2 insertAdjacentHTML 메서드
