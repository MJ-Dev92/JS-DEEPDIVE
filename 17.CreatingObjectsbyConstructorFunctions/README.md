# 17장 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

<aside>
📌 new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

</aside>

```jsx
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Lee";
person.sayHello = function () {
  console.log(`Hi My name is` + this.name);
};

console.log(person); // {name: "Lee", sayHello: f}
person.sayHello(); // Hi My nam is Lee
```

- 생성자 함수란 new 연산자와 함꼐 호출하여 객체(인스턴스)를 생성하는 함수를 말한다.
- 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다.
- 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다. 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.;;

```jsx
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle2.getDiameter()); // 20
```

- 객체는 프로퍼티를 통해 객체 고유의 상태를 표현한다. 그리고 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다.
- 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적이다.
- 객체 고유의 상태 데이터인 radius 프로퍼티의 값은 객체마다 다를 수 있지만 getDiameter메서드는 완전히 동일하다.
- 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 한다. 수십 개의 객체를 생성해야 한다면 문제가 크다.

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

<aside>
📌 생성자 함수에 의한 객체 생성 방식은 마치 객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

</aside>

```jsx
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
}
// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### this

<aside>
📌 this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값, 즉 this바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

</aside>

| 함수 호출 방식       | this가 가리키는 값(this 바인딩)        |
| -------------------- | -------------------------------------- |
| 일반 함수로서 호출   | 전역 객체                              |
| 메서드로서 호출      | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |

- 생성자 함수는 이름 그대로 객체를 생성하는 함수다. 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.**
- 만약 new 연산자와 함꼐 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```jsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 카리킨다.
console.log(radius); // 15
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 **인스턴스를 생성**하는 것과 **생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)**하는 것이다.

```jsx
// 생성자 함수
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
}
// 인스턴스 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
```

- 자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.
- new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 다음과 같은 과정을 거쳐 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한후 암묵적으로 인스턴스를 반환한다.

### 인스턴스 생성과 this바인딩

- 암묵적으로 빈 객체가 생성된다. 이 빈 객체가 바로 생성자 함수가 생성한 인스턴스다.
- 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩 된다. 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 카리키는 이유가 바로 이것이다.
- 이 처리는 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행된다.

### 바인딩

<aside>
📌 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.

</aside>

```jsx
function Circle(radius) {
  // 1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
}
```

### 인스턴스 초기화

- 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
- this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.
- 이처리는 개발자가 기술한다.

```jsx
function Circle(radius) {
  // 1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.

  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
}
```

### 인스턴스 반환

- 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```jsx
function Circle(radius) {
  // 1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.

  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}
// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter:
```

- 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

```jsx
function Circle(radius) {
  // 1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.

  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
}
// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}
```

- 하지만 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

```jsx
function Circle(radius) {
  // 1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.

  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return 100;
}
// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```

- 이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다. 따라서 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

- 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다. 생성자 함수로서 호출한다느 것은 new 연산자와 함께 호출하여 객체를 생성한느 것을 의미
- 함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다. 함수객체는 내부 슬롯과 내부 메서드를 모두 가지고있기 때문이다
- 함수는 객체이지만 일반 객체와는 다르다 **일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.**
- 함수 객체만을 위한 [[Enviroment]], [[FormalParameters]] 등의 내부슬롯과 [[Call]],[[Construct]] 같은 내부 메서들를 추가로 가지고 있다.

```jsx
function foo() {}

// 일반적인 함수로서 호출 [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

- 내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하며, 내부 메서드 [[Construct]]를 갖는 함숫 객체를 constructor, [[Construct]]를 갖지 않는 함수 객체를 non-constructor라고 부른다.
- 함수 객체는 callable이면서 constructor이거나 callable 이면서 non-constructor다. 모든 함수객체는 호출할 수 있ㅈ지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5064f8fd-4df4-43be-bd16-ad44e99bc6b7/Untitled.png)

### 17.2.5 constructor와 non-constructor의 구분

<aside>
📌 자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 constructor와 non-constructor로 구분한다.

</aside>

- constructor : 함수 선언문, 함수 표현식, 클래식(클래식도 함수다)
- non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수

### 17.2.6 new 연산자

- new 연산자와 함꼐 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.
- 함수 객체의 내부 메서드 [[Call]]이 호출되는 것이 아니라 [[Construct]]가 호출된다.
- new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 한다.

```jsx
// 생성자 함수로서 정의하지 않은 일반함수
function add (x, y) {
	return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst)l // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
	return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'admin');
// 함수가 생성한 객체를 반환한다.
console.log(inst) // {name: "Lee", role: "admin"}

```

- 반대로 new연산자 없이 생성자 함수를 호출하면 일반함수로 호출된다. 다시 말해 함수 객체의 내부메서드 [[Construct]가 호출되는 것이 아니라 [[Call]]이 호출된다.

```jsx
function Circle(radius) {
  this.radius = radius;
  getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = new Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of indefined
```

- Circle 함수는 일반 함수로서 호출되었기 때문에 Circle 함수 내부의 this는 전역 객체 window를 가리킨다.
- radius 프로퍼티와 getDiameter 메서드는 전역 객체의 프로퍼티와 메서드가 된다.

### 17.2.7 new target

<aside>
📌 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있다 이러한 위험성을 회피하기 위해 ES6에서는 new.target을 지원한다.

</aside>

- 함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었느지 확인할 수있다.
- **new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.**
