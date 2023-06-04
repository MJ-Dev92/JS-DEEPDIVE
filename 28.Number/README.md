# 28장 Number

<aside>
📌 표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 떄 유용한 프로퍼티와 메서드를 제공한다.

</aside>

## 28.1 Number 생성자 함수

- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.
- Number 생성자 함수에 인수를 전달하지 않고 new연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

```jsx
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

- ES5에서는 [[NumberData]]를 [[PrimitiveValue]]라 불렀다.
- NUmber 생성자 함수의 인수로 숫자를 전다랗면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.

## 28.2 Number 프로퍼티

### 28.2.2 Number.MAX_VALUE

- Number.MAX_VALUE는 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. Number.MAX_VALUE보다 큰 숫자는 Infitnity다.

### 28.2.3 Number.MIN_VALUE

- Number.Min_VALUE는 자바스크립트에서 표현할 수 있는 가장 작은 양수이다. Number.MIN_VALUE보다 작은 숫자는 0이다

### 28.2.6 Number.POSITIVE_INFINITY

- numver.POSITIVE_INFINITY는 양의 무한대를 나타내는 숫자값 Infinity와 같다.

## 28.3 Number 메서드

### 28.3.1 Number.isFinite

- ES6에서 도입된 Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.
