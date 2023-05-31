## 26.1 함수의 구분

- ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다. 즉 **ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.**
- 다시 말해 ES6 이전의 모든 함수는 callabled이면서 constructor다.
- ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다. 이는 혼란스러우며 실수를 유발할 가능성이 있고 성틍에도 좋지 않다.
- 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 구분했다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3eeae38f-5fd3-4253-b4cc-3280aec63d6d/Untitled.png)

## 26.2 메서드

<aside>
📌 **ES6 사양에서 메서드는 메서드 축약표현으로 정의된 함수만을 의미한다.**

</aside>

```jsx
const obj = {
  x: 1,
  // foo는 메서드다.
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

- **ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다**. 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다.
- **ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다**. super키워드를 사용할 수 있다.

## 26.3 화살표 함수

<aside>
📌 화살표 함수는 표현만 간략한 것이 아니라 내부 동작도 기존의 함수보다 간략하다. 특히 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

</aside>

```jsx
const multiply = (x, y) => x * y;
multiply(2, 3); // 6
```

- 화살표 함수는 함수 표현식으로 정의해야 한다. 매개변수가 한 개인 경우 소괄호를 생략할 수 있다.

```jsx
// concise body
const power = (x) => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
//block body
const power = (x) => {
  return x ** 2;
};
```

- 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할 수 있다. 이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.
- 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸주어야 한다.

```jsx
const create = (id, content) => ({ id, content });
create(1, "JavaScript"); // { id: 1, content: "JavaScript"}
```

### 26.3.2 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.
2. 중복된 매개변수 이름을 선언할 수 없다.
3. 화살표 함수는 함수 자체의 this.arguments, super, new.target 바인등을 갖지 않는다.

### 26.3.3 this

- 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리킨다. 그런데 클래스 내부의 모든 코드에는 strict mode가 암묵적으로 적용된다.
- 이와 같은 콜백 함수 내부의 this 문제를 해결하기 위해 ES6 이전에는 다음과 같은 방법을 사용한다.

1. add 메서드를 호출한 prefixer 객체를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다.
2. Array.prototype.map의 두번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다.
3. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩 한다.

- **화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라 한다.**
- 화살표 함수 내부에서 this를 참조하면 일반적인 식별자처럼 스코프 체인을 통해 상위 스코프에서 this를 탐색한다.

```jsx
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () {
  return this.x;
}).bind(this);
```

- 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.

```jsx
// 중첩 함수 foo의 상위 스코프는 즉시 실행 함수다.
// 따라서 화살표 함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}).call({ a: 1 }); // { a: 1}

// bar 함수는 화살표 함수를 반환한다.
// bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar다.
// 하지만 화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 bar 함수가 반환한
// 화살표 함수 내부에서 참조하는 this는 화살표 함수가 아닌 즉시 실행 함수의 this를 가리킨다.
(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call({ a: 1 }); // { a: 1 }
```

- 만약 화살표함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킨다. 전역 함수의 상위 스코프는 전역이고 전역에서 this는 전역 객체를 가리키기 떄문이다.
- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 this를 교체할 수 없고 언제나 상위 스코프의 this 바인딩을 참조한다.

```jsx
const add = (a, b) => a + b;

console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)()); // 3
```

- 메서들르 화살표 함수로 정의하는 것은 피해야 한다. 메서드를 정의할 떄는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

### 26.3.4 super

```jsx
class Base {
	constructor(name) {
		this.name = name;
	}
	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base {
	// 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
	sayHi = () => `${super.sayHi()} how are you doing`?;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

- super는 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드다.
- this와 마찬가지로 클래스 필드에 할당한 화살표 함수 내부에서 super를 참조하면 constructor 내부의 super 바인딩을 참조한다. 위 예제의 경우 Derived 클래스의 constructor는 생략되었지만 암묵적으로 constructor가 생성된다.

### 26.3.5 arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.

```jsx
(function () {
	// 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
	const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1' : 2}
	foo(3, 4);
})1, 2);

// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError
```

- arguments 객체는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.
- 하지만 화살표 함수에서느 arguments 객체를 사용할 수 없다. 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest파라미터를 사용해야한다.

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

- **Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달 받는다.**

```jsx
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

- Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.
