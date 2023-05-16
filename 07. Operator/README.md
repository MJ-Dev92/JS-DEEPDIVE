# 18장 일급 객체

## 18.1 일급객체

다음과 같은 조건을 만족하는 객체를 일급 객체라 한다.

- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

자바스크립트의 함수는 다음 예제와 같이 위의 조건을 모두 만족하므로 일급 객체다.

- 객체는 값이므로 함수는 값과 동일하게 취급할 수 있다 함수는 값을 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수객체로 평가된다.
- 일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며 함수의 반환값으로 사용할 수도 있다는 것이다. 이는 함수형 프로그래밍을 가능케 하는 자바스클비트의 장점 중 하나다.
- 함수는 객체이지만 일반 객체와는 차이가 있다. 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다.
- 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

## 18.2 함수 객체의 프로퍼티

<aside>
📌 함수는 객체다 따라서 함수도 프로퍼티를 가질 수 있다.

</aside>

```jsx
function square(number) {
  return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));
/*
{
	length : {value: 1, writable: false, enumerable: false, configurable: true},
	name : {value: "square", writable: false, enumerable: false, configurable: true},
	arguments : {value: null, writable: false, enumerable: false, configurable: false},
	caller : {value: null, writable: false, enumerable: false, configurable: false},
	prototype : {value: {...}, writable: true, enumerable: false, configurable: false},
}
*/

// __proto__는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, "__proto__")); // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(object.prototype, "__proto__"));
// {get: f, set: f, enumerable: false, configurable: true}
```

- arguments, caller, length, name, prototype 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.
- Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있다.
- 즉 Object.prototype 객체의 **proto** 접근자 프로퍼티는 모든 객체가 사용할 수 있다.

### 18.2.1 arguments 프로퍼티

<aside>
📌 함수 객체의 arguments프로퍼티 값은 arguments 객체다. arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는순회가능한 유사배열 객체이며, 함수 내부에서 지역변수처럼 사용된다. 즉 함수 외부에서는 참조할수 없다.

</aside>

- 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다. 따라서 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않는다.

```jsx
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); //2
console.log(multiply(1, 2, 3)); //2
```

- 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 이후 인수가 할당된다.
- 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우 인수가 전달되지 않은 매개변수는 undefined로 초기화된 상태를 유지한다. 매개변수의 개수보다 인수를 더 많이 전달한 경우 초과된 인수는 무시된다.
- arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다.
- arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사배열 객체다.
- 유사 배열 객체란 length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체를 말한다.
- 유사 배열 객체는 배열이 아니므로 배열 메서들르 사용할 경우 에러가 발생한다.

```jsx
function sum() {
  // arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

- 이러한 번거로움을 해결하기 위해 ES6에서는 Rest파라미터를 도입했다.

```jsx
// ES6 Rest parameter
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

### 18.2.3 length 프로퍼티

함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변서의 개수를 가리킨다.

```jsx
function foo() {
  console.log(foo.length); // 0
}

function bar(x) {
  return x;
}

console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}

console.log(baz.length); // 2
```

- arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수 의 개수를 가리킨다.

### 18.2.4 name 프로퍼티

- name 프로퍼티는 ES5와 ES6에서 동작을 달리하므로 주의하기 바란다. 익명 함수 표현식의 경우 ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖는다. 하지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```jsx
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function () {};
// ES5 : name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6 : name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문 (Function declaration)
function bar() {}
console.log(bar.name); // bar
```

- 12.4.1절 ‘함수 선언문’에서 살펴본 바와 같이 함수 이름과 함수 객체를 가리키는 식별자는 의미가 다르다는 것을 잊지 말기 바란다. 함수를 호출할 떄는 함수 이름이 아닌 함수 객체를 가리키는 식별자로 호출한다.

### 18.2.5 **proto** 접근자 프로퍼티

- **proto**프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다. 내부슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.
- 자세한 내용은 19장에서 알아보자

### 18.2.6 prototype 프로퍼티

<aside>
📌 prototpye 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로터다.

</aside>

- 일반객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototpye프로퍼티가 없다.

```jsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty("prototype"); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty("prototype"); // false
```

- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토 타입 객체를 가리킨다.
