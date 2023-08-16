# 46장 제너레이터와 async/await

## 46.1 제너레이터란?

- ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
- 제너레이터와 일반 함수의 차이는 다음과 같다.
  1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
  2. 제너레이터 하수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
  3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

- 제너레이터 함수는 function\* 키워드로 선언한다. 그리고 하나 이상의 yield 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.

```jsx
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethod() {
    yield 1;
  }
}
```

## 46.3 제너레이터 객체

- 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
- 제너레이터 객체는 next 메서드를 갖는 이터레이터지만 이터레이터에는 없는 return, throw 메서드를 갖는다. 제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.
  - next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드블록을 실행하고, yield된 값을 value 프로퍼티 값으로 , false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
  - return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리적트 객체를 반환한다.

```jsx
function* genFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (3) {
			console.error(e);
		}
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(henerator.return('End!')); // {value: "End!", done: true}
```

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티값으로 갖는 이터레이터 리적트 객체를 반환한다.
- 이처럼 제너레이터 함수 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용하면 비동기 처리를 동기 처리처럼 구현할 수 있다.

## 46.5 제너레이터의 활용

### 46.5.1 이터러블의 구현

- 제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

```jsx
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() {return this;},
		next() {
			[pre, cur] = [cur, pre + cur];
			// 무한 이터러블이므로 done 프로퍼티를 생략한다.
			return {value: cur};
		}
 	};
}());

// infiniteFibonacci는 무한 이터러블이다.
for (const num og infiniteFibonacci) {
	if (num > 10000) break;
		console.log(num) // 1 2 3 5 8 ...2584 4181 6765
}
```

### 46.5.2 비동기 처리

- 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기처럼 구현할 수 있다.

## 46.6 async/await

- 제너레이터를 사용하여 비동기 처리를 동기 처리처럼 동작하도록 구현하면 코드가 무척 장황해지고 가독성이 나쁘다. 제너레이터보다 간단하고 가독성이 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await가 도입 되었다.
- async/await는 프로미스를 기반으로 동작한다. 비동기 처리 결과를 후속 처리할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.

```jsx
const fetch = require('node-fetch');

async function fetchTodo() {
	const url = 'https://jsonplaceholder.typicode.com/todos/1';

	const response = await fetch(url);
	const todo = awaut response.json();
	console.log(todo)
	// {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}

fetchTodo();
```

### 46.6.1 async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다. async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.

```jsx
// async 함수 선언문
async function foo(n) {
  return n;
}

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3
```

### 46.6.2 await 키워드

- await 키워드는 프로미스가 settled상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.

```jsx
const fetch = require('node-fetch');

const getGithubUserName = async id => {
	const res = awit fetch(`https://api.github.com/users/${id}`); // 1
	const { name } = await res.json(); // 2
	console.log(name); // Ungmo Lee
};

getGithubUserNae('ungmo2');
```

- 1의 fetch 함수가 수행한 HTTP 요총에 대한 서버의 응답이 도착해서 fetch 함수가 반환한 프로미스가 settled 상태가 될 때까지 1은 대기하게 된다.
- 이후 **프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다.**

### 46.6.3 에러 처리

- async/await에서 에러 처리는 try…catch 문을 사용할 수 있다. 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 떄문에 호출자가 명확하다.
- **async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.**
- 따라서 async 함수를 호출하고 Promise.prototype.catch 후속 처리 메서드를 사용해 에러를 캐치할 수도 있다.
