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

<aside>
📌 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb76705b-c0de-4fc0-b53d-d93c0a9027a8/Untitled.png)

- 접근자 함수는 getter / setter 함수라고도 부른다. 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할수 도 있고 하나만 정의할 수도 있다.

```jsx
const person = {
  // 데이터 프로퍼티
  firstName: "Ungmo",
  lastName: "Lee",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당 : '31.1 배열 디스트럭처링 할당' 참고
    [this.firstName, this.lastName] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + " " + person.lastName); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = "Heegun Lee";
console.log(person); // {firstName: 'Heegun', lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 ㄱ밧의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// firstName은 데이터 프로퍼티다.
// 데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]
// 프로퍼티 어트리뷰트를 갖는다.
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
// {value: 'Heegun', writable: true, enumerable: true, configurable: true}

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]]
// 프로퍼티 어트리뷰트를 갖는다.
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}
```

- getter / setter 함수의 이름 fullName이 접근자 프로퍼티다. 접근자 프로퍼티는 자체적으로 값을 가지지 않으며 다만 데이터 프로퍼티의 값을 읅거나 저장할 때 관여할 뿐이다.
- 내부 슬롯/메서드 관점에서 설명하면 접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]] 내부 메서드가 호출되어 다음과 같이 동작한다.

1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야 한다. 프로퍼티 키 ‘fullName’은 문자열이므로 유효한 프로퍼티 키다.
2. 프로토타입 체인에서 프로퍼티를 검색한다. person 객체 fullName 프로퍼티가 존재한다.
3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다. fullName 프로퍼티는 접근자 프로퍼티다.
4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값 즉 getter 함수를 호출하여 그 결과를 반환한다. 프로펕티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get프로퍼티 디스크립터 객체의 get프로퍼티 값과 같다.

## 16.4 프로퍼티 정의

<aside>
📌 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

</aside>

- 프로퍼티 디스크립터 객체에서 생략된 어트리뷰트는 기본값이 적용된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9e814b8-b858-4511-9b2c-8344ba063ee1/Untitled.png)

## 16.5 객체 변경 방지

- 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9f79843d-0298-4ab3-845c-156549e081b3/Untitled.png)

### 16.5.1 객체 확장 금지

- Object.preventExtensions 메서드는 객체의 확장을 금지한다. 객체 확장 금지란 프로퍼티 추가 금지를 의미한다 즉, **확장이 금지된 객체는 프로퍼티 추가가 금지된다.**
- 프로퍼티는 프로퍼티 동적 추가와 Object.defineProperty 메서드로 추가할 수 있다, 이 두 가지 추가 방법이 모두 금지된다.

### 16.5.2 객체 밀봉

- Object.seal 메서드는 객체를 밀봉한다. 객체 밀봉이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 의미한다. 즉 **밀봉된 객체는 읽기와 쓰기만 가능하다.**
- 밀봉된 객체인지 여부는 Object.isSealed 메서드로 확인할 수 있다.

### 16.5.3 객체 동결

- Object.freeze 메서드는 객체를 동결한다. 객체 동결이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지를 의미한다. 즉 동결된 객체는 읽기만 가능하다.
- 동결된 객체인지 여부는 Object.isFrozen 메서드로 확인할 수 있다.

### 16.5.4 불변객체

- 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩객체까지는 영향을 주지는 못한다. 따라서 Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다.
- 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.
