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

- sdsd
