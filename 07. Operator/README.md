# 19장 프로토타입

<aside>
📌 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 “모든 것”이 객체다. 원시 타입의 값을 제외한 나머지 값들(함수, 배열, 정규 표현식 등)은 모두 객체다.

</aside>

## 19.1 객체지향 프로그래밍

<aside>
📌 객체지향 프로그래밍은 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그리밍 패러다임을 말한다.

</aside>

- 객체향 프로그래밍은 실세계의 실체를 인식하는 철학적 사고를 ㅡㅍ로그래밍에 접목하려는 시도에서 시작한다. 실체는 특징이나 성질을 나타내는 **속성**을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.
- 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 추상화라 한다.

```jsx
// 이름과 주소 속성을 갖는 객체
const person = {
  name: "Lee",
  address: "seoul",
};

console.log(person); // {name: "Lee", address: "seoul"}
```

- 프로그래머(subject, 주체)는 이름과 주소 속성으로 표현된 객체인 person을 다른 객체와 구별하여 인식할 수 있다. 이처럼 **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**를 객체라 하며, 객체지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.
- 원에는 반지름이라는 속성이 있다. 반지름을 가지고 원의 지름, 둘레, 넓이를 구할 수있다. 이때 반지름은 원의 **상태를 나타내는 데이터**이며 원의 지름, 둘레, 넓이를 구하는 것은 **동작**이다.

```jsx
const circle = {
  radius: 5, // 반지름
  // 원의 지름: 2r
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레: 2pir
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이: pirr
  getArea() {
    return Math.PI * this.radius ** 2;
  },
};

console.log(circle);
// {radius: 5, getDiameter: f, getPerimeter: f, getArea: f}

console.log(circle.getDiameter()); // 10
console.log(circle.getPerimeter()); // 31.41592....
console.log(circle.getArea()); // 78.53981633....ㅊㅇㅁㄴㅇㅇ
```

- 이처럼 객체지향 프로그래밍은 객체의 **상태**를 나타내는 데어타와 상태 데이터를 조작할 수 있는 **동작**을 하나의 논리적인 단위로 묶어 생각한다.
- 따라서 객체는 **상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조**라고 할 수 있다.
- 이떄 객체의 상태 데이터를 프로퍼티, 동작을 메서드라 부른다.

## 19.2 상속과 프로토타입

상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

- 자바크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.
- 중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용하는 것이다.

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592535...
console.log(circle2.getArea()); // 12.56637061...
```

- getArea 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용하므로 단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
- Circle생성자 함수는 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성하고 무든 인스턴스가 중복 소유한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66d34bdc-c9f6-4ee2-b1a9-7ab57bc66ccb/Untitled.png)

- 동일한 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비한다.
- 또한 인스턴스를 생성할 때마다 메서드를 생성하므로 퍼포먼스에도 악영향을 준다. 만약 10개의 인스턴스를 생성하면 내용이 동일한 메서드도 10개 생성된다.
- 상속을 통해 불필요한 중복을 제거해 보자. **자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴사가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역학을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속 받는다.
// 즉 Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // trye

console.log(circle1.getArea()); // 3.141592535...
console.log(circle2.getArea()); // 12.56637061...
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04fee354-4f84-4723-bd8a-b283fa647545/Untitled.png)

- Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.
- Circle생성자 함수가 생성하는 모든 인스턴스는 getArea 메서드를 상속받아 사용할 수 있다.
- 즉 자신의 상태를 나타내는 radius프로퍼티만 개별적으로 소유하고 내용이 동일한 메서드는 상속을 통해 공유하여 사용하는 것이다.
- 상속은 코드의 재사용이란 관점에서 매우 유용하다. 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현 없이 상위 객체인 프로토타입의 자산을 공유하여 사용할 수 있다.

## 19.3 프로토타입 객체

<aside>
📌 프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다. 프로토타입은 어떤 객체의 상위 객체의 역할을 하느 객체로서 다른 객체에 공유 프로퍼티를 제공한다. 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

</aside>

- 객체가 생성될때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.
- 객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype이고 생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.
- 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다. 객체와 프로토타입과 생성자 함수는 다음 그림과 같이 서로 연결되어 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/914138d4-76e9-4017-bfe5-8aba74a95a12/Untitled.png)

- 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype프로퍼티를 통해 프로토타입에 접근할 수 있다.

### 19.3.1 **proto** 접근자 프로퍼티

<aside>
📌 **모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.**

</aside>

### **proto**는 접근자 프로퍼티다.

- 내부 슬롯은 프로퍼티가아니다. 내부 슬롯에는 직접 접근할 수 없으며 **proto** 접근자 프로퍼티를 통해 간접적으로 [[Prototype]]내부 슬롯의 값, 즉 프로토타입에 접근할 수 있다.

```jsx
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter 함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

### **proto** 접근자 프로퍼티는 상속을 통해 사용된다.

<aside>
📌 __proto__ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다. 모든 객체는 상속을 통해 Object.prototype.__proto__ 접근자 프로퍼티를 사용할 수 있다.

</aside>

```jsx
const person = { name: "Lee" };

// person 객체는 __proto__프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty("__proto__")); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
// {get: f, set: f, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

### **proto** 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

<aside>
📌 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

</aside>

- 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다. 즉 프로퍼티 검색 방향이 한쪽 방향으로만 흘러가야한다.
- 순환 참조하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 앟기 떄문에 프로토타입 체인에서 프로퍼티를 검색할 때 무한루프에 빠진다.
- 따라서 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 **proto** 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

### **proto** 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

- 모든 객체가 **proto** 접근자 프로퍼티에 사용할 수 있는 것은 아니기 때문에 접근자 프로퍼티를 직접 사용하는 것은 권장하지 않는다.
- 나중에 살펴보겠지만 직접 상속을 통해 Object.prototype을 상속받지 않는 객체를 생성할 수도 있기 떄문에 **proto** 접근자 프로퍼티를 사용할 수 없는 경우가 있다.

```jsx
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null)l
// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefirned

// 따라서 __proto__ 보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

- 따라서 **proto** 접근자 프로퍼티 댁신 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf를 사용하고, 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 사용하는 것을 권장한다.

```jsx
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj);
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 19.3.2 함수 객체의 prototype 프로퍼티

<aside>
📌 **함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

</aside>

- prototype 프로퍼티는 생성자 함수가 생성할 객체의 프로토타입을 가리킨다. 따라서 생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor인 화살표 함수와 ES6메서드 축약 표현으로 정의한 메서드는 prototype프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.
- 모든 객체가 가지고있는 (엄밀히 말하면 Object.prototype으로부터 상속받은) **proto** 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

<aside>
📌 모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다. 이 연결은 생성자 함수가 생성될때, 즉 함수 객체가 생성될 떄 이뤄진다.

</aside>

19.4 리터럴 표기벅에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다. 이때 constructor프로퍼티가 가리키는 생성자 함수는 인스턴스르 생성한 생성자 함수다.

```jsx
// obj 객체를 생성한 생성자 함수는 Object다.
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Finction("a", "b", "return a + b");
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person("Lee");
console.log(me.constructor === Person); // true
```

- 리터럴 표기법에의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함꼐 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```jsx
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
  return a + b;
};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규 표현식 리터럴
const regexp = /is/gi;
```

- 리터럴 표기법에 의해 생서된 객체도 물론 프로토타입이 존재한다. 하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.

```jsx
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

- Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로는 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

### 추상 연산

<aside>
📌 추상연산은 ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것이다. ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드라고 이해하자.

</aside>

```jsx
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor == Function); // true
```

- 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다.
- 프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 떄문이다.
- 다시말해 **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**
- 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 크게 무리는 없다. 리터러 표기법에 의해 생성된 객체의 생성자 함수와 프로토 타입은 같다.

## 19.5 프로토타입의 생성 시점

<aside>
📌 객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

</aside>

### Object.create메서드와 클래스에 의한 객체 생성

<aside>
📌 아직 살펴보지 않았지만 Object.create 메서드와 클래스로 객체를 생성하는 방법도 있다. Object.create 메서드와 클래스로 생성한 객체도 생성자 함수와 연결되어 있다.

</aside>

- **프로토 타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**
- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

- [[Construct]]를 갖는 함수 객체는 new 연산자와 함꼐 생성자 함수로서 호출할 수 있다.
- 생성자 함수로서 호출할 수 있는 함수 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

```jsx
// 함수 정의 (constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: f}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

- 생성자 함수로서 호출할수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.
- 생성된 프로토타입은 Person 생성자 함수의 prototype 프로퍼티에 바인딩된다. Person 생성자 함수와 더불어 생성된 프로토타입의 내부를 살펴보자.
- 생성된 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체다. 프로토타입도 객체이고 모든 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 갖는다. 생성된 프로토타입의 프로토타입은 Object.prototype이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4209a9f6-a4db-4e7a-b7fa-62f192bd74f8/Untitled.png)

- 이처럼 빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성시점

- Object, String, Number, Function, Array, RegExp, Date Promise등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
- 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.
- 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

### 전역 객체

<aside>
📌 전역 객체는 코드가 실해되기 이전 단계에 자바스크립트 엔진에 의해 생성되는 특수한 객체다. 전역 객체는 클라이언트 사이드 환경(브라우저)에서는 window, 서버 사이드 환경(Node.js)에서는 global 객체를 의미한다.

전역 객체는 표준 빌트인 객체들과 환경에 따른 호스틑 객체(클라이언트 Web API 또는 Node.js의 호스트 API)그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다. Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 생성자 함수다.

</aside>

- 이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. **이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당된다.**
- 이로써 생성된 객체는 프로토타입을 상속받는다.

### 19.6 객체 생성 방식과 프로토타입의 결정

<aside>
📌 객체는 다음과 같이 다양한 생성 방법이 있다.

</aside>

- 객체 리터럴,
- Object 생성자 함수
- 생성자 함수
- Object.create메서드
- 클래스

<aside>
📌 이처럼 다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

</aside>

- 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

<aside>
📌 자바스크립트 엔진은 객체 리터러을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출한다. 이때 추상연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

</aside>

- 즉 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```jsx
const obj = { x: 1 };
```

- 위 객체리터럴이 평가되면 추상 연산 OrdinaryObjecctCreate에 의해 다음과 같이 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어진다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6bdcf6d4-87c6-4086-9dee-246dd075deb1/Untitled.png)

- object.prototype을 상속 받음으로써 obj 객체는 constructor 프로퍼티와 hasOwnProperty메서드 등을 소유하지 않았지만 자신의 자산인 것처럼 자유롭게 사용할 수 있다.
- 이는 obj객체가 자신의 프로토타입인 Object.prototype 객체를 상속받았기 때문이다.

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

<aside>
📌 Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성된다. Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 OrdinaryObejctCreate가 호출된다. 이때 추상 연산 OrdinaryObejctCreate에 전달되는 프로토타입은 Object.prototype이다.

</aside>

- 즉 Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```jsx
const obj = new Object();
obj.x = 1;
```

- 코드가 실행되면 추상 연산 OrdinaryObjectCreate에 의해 다음과 같이 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어진다. 객체 리터럴에 의해 생성된 객체와 동일한 구조를 갖는 것을 알 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/777f1577-3e05-4869-816d-11e8e98e9909/Untitled.png)

- 이처럼 Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받는다.
- 객체 리털러과 Object 생성자 함수에 의한 객체 생성 방식의 차이는 프로퍼티를 추가하는 방식에 있다.
- 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티를 추가해야 한다.

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

## 19.7 프로토타입 체인

```jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new person("Lee");

// hasownProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty("name")); // true
```

- Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드인 hasOwnProperty를 호출할 수 있다.
- 이것은 me 객체가 Person,prototype뿐만 아니라 Object.prototype도 상속받았다는 것을 의미한다.
- me 객체의 프로토타입은 Person.prototype이다.

```jsx
Object.getPrototypeOf(me) === Person.prototype; // true
```

- person.prototype의 프로토타입은 Object.prototype이다. 프로토타입의 프로토타입은 언제나 Object.prototype이다.

```jsx
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad45eb1b-2a26-4c10-a9f2-6defc3e7d060/Untitled.png)

- **자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 떄 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.**
- **이를 프로토타입 체인이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**

```jsx
// hasOwnProperty는 Object.prototype의 메서드다.
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메서드를 검색하여 사용한다.
me.hasOwnProperty("name"); // true
```

- me.hasOwnProperty(’name’)과 같이 메서드를 호출하면 다음과 같은 과정을 거쳐 메서드를 검색한다.

1. 먼저 hasOwnProperty메서드를 호출한 me 객체에서 hasOwnProperty 메서드를 검색한다. me 객체에는 hasOwnProperty메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 [[Prototype]] 내부 슬롯에 바인딩되어 있는 프로토타입으로 이동하여 hasOwnProperty메서드를 검색한다.
2. Person.prototype에도 hasOwnProperty 메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 [[Prototype]]내부 슬롯에 바인딩되어 있는 프로토타입으로 이동하여 hasOwnProperty메서드를 검색한다.
3. Object.prototype에는 hasOwnProperty메서드가 존재한다. 자바스크립트 엔진은 Object.prototype.hasOwnProperty 메서드를 호출한다. 이때 Object.prototype.hasOwnProperty메서드의 this에는 me 객체가 바인딩 된다.

```jsx
Object.prototype.hasOwnProperty.call(me, "name");
```

### call 메서드

<aside>
📌 call 메서드는 this로 사용할 객체를 전달하면서 함수를 호출한다. 나중에 자세히 살펴볼 것이기 떄문에 지금은 this로 사용할 me 객체를 전달하면서 Object.prototype.hasOwnProperty 메서드를 호출한다고 이해하자

</aside>

- 프로토 타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이다. 따라서 모든 객체는 Object.prototype을 상속받는다.
- **Object.prototype을 프로토타입 체인의 종점**이라 한다. [[Prototype]] 내부 슬롯의 값은 null이다.
- 프로토타입 체인의 종점인 Object.prototype에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다. 이때 에러가 발생하지 않는 것에 주의하자.

```jsx
console.log(me.foo); // undefined이
```

- 이처럼 자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티 / 메서드를 검색한다.
- 자바스크립트 엔진은 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다.
- **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘**이라고 할 수있다.
- 이에 반해, 프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다.
- 자바스크립트 엔진은 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다.
- 따라서 **스코프체인은 식별자 검색을 위한 메커니즘**이라고 할 수 있다.

```jsx
me.hasOwnProperty("name");
```

- ㅇ위 예제의 경우 먼저 스코프 체인에서 me 식별자를 검색한다. me 식별자는 전역에서 선언되었으므로 전역 스코프에서 검색된다.
- 식별자를 검색한 다음 me 객체의 프로토타입 체인에서 hasOwnPropery메서드를 검색한다.
- 이처럼 **스코프체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.**

## 19.8 오버라이딩과 프로퍼티 섀도잉
