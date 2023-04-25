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
