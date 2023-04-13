# 16장 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

<aside>
📌 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드다. ECMAScript 사양에 등장하는 이중 대괄화 ([[…]])로 감싼 이름들이 내부 슬롯과 내부 메서드다.

</aside>

- 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 내부 로직이므로 원칙적으로 자바스크립트는 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않는다.
- 일부 내부 슬롯과 내부 메서드에 한하여 직접적으로 접근할 수 있는 수단을 제공한다.
- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. 원칙적으로 직접 접근할 수 없지만 [[Prototype]] 내부 슬롯의 경우, **proto** 를 통해 간접적으로 접근할 수 있다.

```jsx
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // -> Uncaught SyntaxError
// 단 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
o.__proto__ // -> object.prototype
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- **자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.**
- 프로퍼티의 상태란 프로퍼티의 값, 값의 갱신 기능 여부, 열거 가능 여부,재정의 가능 여부를 말한다.
- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값 meta-property인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다.
- 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수 있다.

```jsx
const person = {
  name: "Lee",
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

- Object.getOwnPropertyDescriptor 메서드를 호출할 때 첫 번째 매개변수에는 객체의 참조를 전달하고, 두번째 매개변수에는 프로퍼티 키를 문자열로 전달한다.
- 이때 Object.getOwnPropertyDescriptor 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 **프로퍼티 디스크립터객체**를 반환한다.
- 만약 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 undefined가 반환된다.
- ES8에서 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.

```jsx
const person = {
  name: "Lee",
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
	{value: "Lee", writable: true, enumerable: true, configurable: true}
	{value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

<aside>
📌 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.

</aside>

- 데이터 프로퍼티
  - 키와 값으로 구성된 일반적인 프로퍼티다. 지금까지 살펴본 모든 프로퍼티는 데이터 프로퍼티다.
- 접근자 프로퍼티
  - 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

### 16.3.1 데이터 프로퍼티

- 데이터 프로퍼티는 프로퍼티 어트리뷰트를 갖는다. 이 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1617e7f-7fb6-41f4-82cf-7754b084f4a8/Untitled.png)

### 16.3.2 접근자 프로퍼티

- 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.
