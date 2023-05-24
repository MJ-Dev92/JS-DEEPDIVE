# 25장 클래스

# 25장 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- 자바스크립트는 프로토타입 기반 객체지향 언어다. 비록 다른 객체 지향 언어와의 차이점에 대한 논쟁이 있긴 하지만 자바스크립트는 강력한 객체지향 프로그래밍 능력을 지니고 있다.
- 프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어다 ES5에서는 클래스 없이도 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.
- 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕이라고 볼 수도 있다.
- 클래스와 생성자 함수 차이점이 몇 가지가 있다.
  1. 클래스를 new 연선자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수는 new 연산자 없이 호출하면 일반 함수로서 호출된다.
  2. 클래스는 상속을 지원하는 extends와 super 키우ㅗ드를 제공한다. 하지만 생성자 함수는 extends와 super키워드를 지원하지 않는다.
  3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
  4. 클래스 내의 모든 코드에는 암무적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다 하지만 생성자 함순느 암묵적으로 strict mode가 지정되지 않는다.
  5. 클래스의 constructor, 프로토타입 메서드 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 다시말해 열거되지 않는다.
- 클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료하다. 따라서 클래스를 프로토타입 기반 객체생성 패턴의 단순한 문법적 설탕이라고 보기보다는 새로운 객체 생성 메커니즘으로 보는 것이 좀더 합당하다.

## 25.2 클래스 정의

<aside>
📌 클래스는 class키워드를 사용하여 정의한다. 클래스 이름은 생성자 함수와 마찬가지로 파스칼 케이스를 사용하는 것이 일반적이다. 파스칼 케이스를 사용하지 않아도 에러가 발생하지 않는다.

</aside>

```jsx
// 클래스 선언문
class Person {}
```

- 클래스는 함수와 마찬가지로 이름을 가질 수도 있고, 갖지 않을 수도 있다.

```jsx
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

- 클래스는 일급 객체로서 다음과 같은 특직을 갖는다
  - 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  - 변수난 자료구조에 저장할 수 있다.
  - 함수의 매개변수에게 전달할 수 있다.
  - 함수의 반환값으로 사용할 수 있다.
- 클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세가지가 있다.

```jsx
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log("Hello");
  }
}

// 인스턴스 생성
const me = new Person("Lee");

// 인스턴스의 프로퍼티 참조
console.log(me.naem); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

## 25.3 클래스 호이스팅

```jsx
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. 이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수 즉 constructor다. 생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불처 생성된다.

```jsx
console.log(Person);
// ReferenceError

// 클래스 선언문
class Person {}
```

- 클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않다.

```jsx
const Person = "";

{
  // 호이스팅이 발생하지 않는다면 ''이 호출되어야 한다.
  console.log(Person);
  // ReferenceError: Canot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```

- 클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다. 단 클래스는 let, const 키워드로 선언한 변수처럼 호이스팅이된다. 따라서 클래스 선언문 이전에 일시적 사각지대에 빠지기 떄문에 호이스팅이 발생하지 않는 것처럼 동작한다.

## 25.4 인스턴스 생성

<aside>
📌 클래스는 생성자 함수이며 new 연산자와 함꼐 호출되어 인스턴스를 생성한다.

</aside>

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

- 함수는 new 연산자의 사용 여부에 따라 일반함수로 호출되거나 인스턴스 생성을 위한 생성자 함수로 호출되지만 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new연산자와 함께 호출해야 한다.

```jsx
class Person {}

// 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const me = Person();
// TypeError
```

## 25.5 메서드

<aside>
📌 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세 가지가 있다.

</aside>

### 25.5.1 constructor

<aside>
📌 constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. constructor는 이름을 변경할 수 없다.

</aside>

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

- 클래스는 인스턴스를 생성하기 위한 생성자 함수다. 클래스의 내부를 들여다보자
- 클래스는 평가되어 함수 객체가 된다. 함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프체인을 구성한다.
- 모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있다. 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미한다.

```jsx
// 클래스
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}

// 생성자 함수
function Person(name) {
  // 인스턴스 생성 및 초기화
  this.name = name;
}
```

- 클래스가 생성한 인스턴스 어디에도 constructor 메서드가 보이지 않는다. 이는 클래스 몸체에 정의한 constructor가 단순한 메서드가 아니라는 것을 의미한다.
- constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 다시말해 클래스 정의가 평가되면 construcotr의 기술된 동작을 하는 함수 객체가 생성된다.
- constructor는 생성자 함수와 유사하지만 몇 가지 차이가 있다.
- constructor는 클래스 내에 최대 한 개만 존재할 수 있다. 만약 클래스가 2개 이상의 constructor를 포함하면 문법 에러가 발생한다.

### 25.5.2 프로토타입 메서드

생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 다음과 같이 명시적으로 프로토타입에 메서드를 추가해야한다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

- 클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서들르 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

### 25.5.3 정적 메서드

```jsx
class Person {
  // 생성자
  constructor(name) {
    //인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}
```

- 정적 메서드는 클래스에 바인딩된 메서드가 된다. 클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다.
- 클래스는 클래스 정의(클래스 선언문이나 클래스 표현식)가 평가되는 시점에 함수 객체가 되므로 인스턴스와 달리 별다른 생성 과정이 필요 없다. 따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다.

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

<aside>
📌 정적 메서드와 프로토타입 메서드는 무엇이 다르며, 무엇을 기준으로 구분하여 정의해야 할지 생각해 보자. 정적 메서드와 프로토타입 메서드의 차이는 다음과 같다.

</aside>

1. 정적 메서드와 프로토타입 메서든느 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서든느 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서든느 인스턴스 프로퍼티를 참조할 수 있다.

### 25.5.5 클래스에서 정의한 메서드의 특징

<aside>
📌 클래스에서 정의한 메서드는 다음과 같은 특징을 갖는다.

</aside>

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 떄는 콤마가 필요 없다.
3. 암묵적으로 srict mode로 실행된다.
4. for…in 문이나. Object.keys메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열겨 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함꼐 호출할 수 없다.

## 25.6 클래스의 인스턴스 생성 과정

<aside>
📌 new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드[[Construct]]가 호출된다. 클래스는 new 연산자 없이 호출할 수 없다.

</aside>

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

<aside>
📌 인스턴스 프로퍼티는 constructor 내부에 정의해야 한다.

</aside>

```jsx
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public 하다.
  }
}

const me = new Person("Lee");

// name은 public하다.
console.log(me.name); // Lee
```

- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.
- ES6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다.

### 25.7.2 접근자 프로퍼티

<aside>
📌 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 떄 사용하는 접근자 함수로 구성된 프로퍼티다.

</aside>

- getter는 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용하며, 참조 시에 내부적으로 getter가 호출된다.
- setter도 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용하며, 할당 시에 내부적으로 setter가 호출된다.
- 클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 따라서 클래스 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.

### 25.7.3 클래스 필드 정의 제안

<aside>
📌 클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다

</aside>

- 자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있다. 따라서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러가 발생한다.

```jsx
class Peroson {
  // 클래스 필드 정의
  name = "Lee";
}

const me = new Person("Lee");
console.log(me); // Person {name "Lee"}
```

- 클래스 몸체에서 클래스 필드를 정의할 수 있는 클래스 필드 정의제안은 아직 정식 표준 사양으로 승급되지 않았다.
- 하지만 최신 브라우저(Chrome 72이상)와 최산 Node.js(버전 12 이상)는 표준 사양으로 승급이 확실시되는 이 제안을 선제적으로 미리 구현해 놓았다.
- 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩해서는 안 된다. this는 클래스의 constructor와 메서드 내에서만 유효하다.
- 클래스 필드를 참조하는 경우 자바스크립트에서는 this를 반드시 사용해야 한다.
- 클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다.
- 인스턴스를 생성할 때 외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면 constructor에서 클래스 필드를 초기화해야 한다.
- 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문에 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.
- 인스턴스 프로퍼티를 정의하는 방식은 두 가지가 되었다. 인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 있다면 constructor에서 인스턴스 프로퍼티를 정의하는 기존 방식을 사용하고, 인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 없다면 기존 constructor에서 인스턴스 프로퍼티를 정의한느 방식과 클래스 필드 정의 제안 모두 사용할 수 있다.

### 25.7.4 private 필드 정의 제안

<aside>
📌 자바스크립트는 캡슐화를 완전하게 지원하지 않는다. 인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. 즉 언제나 public이다.

</aside>

```jsx
class Person {
  constructor(name) {
    this.name = name; // 인스턴스 프로퍼티는 기본적으로 public하다
  }
}

const me = new Person("Lee");
console.log(me.name); // Lee
```

- 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로 노출된다.

```jsx
class Person {
  name = "Lee"; // 클래스 필드도 기본적으로 public하다.
}

const me = new Person();
console.log(me.name); // Lee
```

- 현재 private필드를 정의할 수 있는 새로운 표준 사양이 제안 되어있다. private 필드의 선두에는 #을 붙여준다. private필드를 참조할 떄도 #을 붙여주어야 한다.

```jsx
class Person {
  // private 필드 정의
  #name = "";

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person("Lee");

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxEerror
```

- public필드는 어디서든 참조할 수 있지만 private 필드는 클래스 내부에서만 참조할 수 있다.
- 이처럼 클래스 외부에서 private 필드에 직접 접근할 수 있는 방법은 없다. 다만 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.

### 25.8 상속에 의한 클래스 확장

<aside>
📌 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.

</aside>

- 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만 생성자 함수는 그렇지 않다.

### 25.8.2 extends 키워드

<aside>
📌 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.

</aside>

```jsx
// 수퍼 클래스
class Base {}

// 서브 클래스
class Derived extends Base {}
```

- 상속을 통해 확장된 클래스를 서브클래스라고 부르고, 서브클래스에게 상속된 클래스를 수퍼클래스라 부른다.
- extends 키워드의 역할은 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것이다. 클래스도 프로토타입을 통해 상속 관계를 구현한다.

  25.8.3 동적 상속

- extends 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다.
- extends 키워드 다음에는 클래스뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

```jsx
function Base1() {}

class Base2 {}

let condition = true;

class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### 25.8.4 서브클래스의 constructor

- 서브클래스에서 constructor를 생략하면 클래스에 다음 같은 constructor가 암묵적으로 정의된다. args는 new 연산자와 함꼐 클래스를 호출할 때 전달한 인수의 리스트다.

```jsx
class Base {
	constructor() {}
}

// 서브클래스
class Derived extends Base {
	constructor(...args) { super(...args); }
}

const derived = new Derived();
console.log(derived); Derived {}
```

- 수퍼클래스와 서브클래스 모두 constructor를 생략하면 빈 객체가 생성된다. 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야한다.

### 25.8.5 super 키워드

<aside>
📌 super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.

</aside>

- super를 호출하면 수퍼클래스의 constructor를 호출한다.
- super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

### super 호출

- **super를 호출하면 수퍼클래스의 constructor를 호출한다.**
- new연산자와 함께 서브클래스를 호출하면 전달한 인수는 모두 서브클래스에 암묵적으로 정의된 constructor의 super 호출을 통해 수퍼클래스의 constructor에 전달된다.

```jsx
// 수퍼클래스
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  // 다음과 같이 암묵적으로 constructor가 정의된다.
  // constructor(...args){ super(...args); }
}

const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}
```

- new연산자와 함꼐 서브클래스를 호출하면서 전달한 인수 중에서 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스 constructor에서 호출하는 super를 통해 전달한다.
- super를 호출할 떄 주의할 사항은 다음과 같다.
  1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에는 반드시 super를 호출해야한다.
  2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
  3. super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor난 함수에서 super를 호출하면 에러가 발생한다.

### super 참조

**메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.**

1. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킨다.
   - super는 자신을 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 가리킨다.
   - call 메서드를 사용해 this를 전달해야한다. 만약 call 메서드를 사용하지 않고 호출하면 Base.prototype.sayHi 메서드 내부의 this는 Base.prototype을 가리킨다.
   - 이를 위해 메서드는 내부 슬롯 [[HomeObject]]를 가지며, 자신을 바인딩하고 있는 객체를 가리킨다.
   - **주의할 것은 ES6의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]]를 갖는다는 것이다.**
2. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킨다.

### 25.8.6 상속 클래스의 인스턴스 생성과정

1. **서브클래스의 super호출**
   - 자바스크립트 엔진은 클래스를 평가할때 수퍼 클래스와 서브클래스를 구분하기 위해 base, derived 값으로 갖는 내부 슬롯을 갖는다. 상속 받지 않는 클래스 값은 base 상속 받는서브클래스는 내부 슬롯의 값이 derived다.
   - **서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다. 이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야하는 이유다.**
2. **수퍼클래스의 인스턴스 생성과 this 바인딩**
   - 인스턴스는 수퍼클래스가 생성하는 것이다. 하지만 new 연산자와 함께 호출된 클래스가 서브클래스라는 것이 중요하다. 즉 new 연산자와 함께 호출된 함수를 가리키는 new.target은 서브클래스를 가리킨다. 따라서 **인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리된다.**
3. **수퍼클래스의 인스턴스초기화**
4. **서브클래스 constructor로의 복귀와 this 바인딩**
5. **서브클래스의 인스턴스 초기화**
6. **인스턴스 반환**
