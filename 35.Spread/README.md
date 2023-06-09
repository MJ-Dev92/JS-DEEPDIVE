# 35장 스프레드 문법

<aside>
📌 스프드 문법은 하나로 뭉쳐있는 여려 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

</aside>

```jsx
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다(1, 2, 3)
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(...'Hello'); // H e l l o

// Map과 Set은 이터러블이다.
console.log(...new Map(['a', '1'],['b', '2'])); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Map([1, 2, 3]); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
// TypeError
```

- 스르레드 문법은 쉼표로 구분한 값의 목록을 사용한는 문맥에서만 사용할 수 있다.
  - 함수 호출문의 인수 목록
  - 배열 리터럴의 요소 목록
  - 객체 리터럴의 프로퍼티 목록

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

```jsx
const arr = [1, 2, 3];

// 배열 arr의 요소 중에서 최대값을 구하기 위해 Math.max를 사용한다.
const max = Math.max(arr); // NaN
```

```jsx
const arr = [1, 2, 3];

const max = Math.max(...arr); // 3
```

- 스프레드 문법은 앞에서 살펴본 Rest 파라미터와 형태가 동일하여 혼동할 수 있다.
- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 배개변수 이름 앞에 …을 붙이는 것이다.
- 스프레드 문법은 여러 개의 값이 하나로 뭉쳐있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이다. 따라서 Rest 파라미터와 스프레드 문법은 서로 반대 개념이다.

```jsx
// Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
  console.log(rest); // 1, 2, 3 -> [1, 2, 3]
}
// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

## 35.2 배열 리터럴 내부에서 사용하는 경우

### 35.2.1 concat

- ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우 배열 리터럴만으로 해결할 수 없고 concat 메서드를 사용해야 한다.

```jsx
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]
```

```jsx
// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

### 35.2.2 splice

```jsx
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

// 세 번째 인수 arr2를 해체하여 전달해야 한다.
// 그렇지 않으면 arr1에 arr2 배열 자체가 추가된다.
arr1.splice(1, 0, arr2);

// 기대한 결과는 [1, [2, 3], 4]가 아니라 [1, 2, 3, 4]다.
console.log(arr1); // [1, [2, 3], 4]
```

```jsx
// ES6
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

### 35.2.3 배열 복사

```jsx
// ES6
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

- 이떄 원본 배열의 각 요소를 얕은 복사하여 새로운 복사본을 생성한다, 이는 slice 메서드도 마찬가지다.

### 35.2.4 이터러블을 배열로 변환

```jsx
// Rest 파라미터 args는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

console.log(sum(1, 2, 3)); // 6
```

- 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에서 도입된 Array.from 메서드를 사용한다.

```jsx
// Array.form은 유사 객체 배열 또는 이터러블을 배열로 변환한다.
Array.from(arrayLike); // [1, 2, 3]
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

<aside>
📌 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다. 스프레드 문법의 대상은 이터러블이어야 하지만 스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다.

</aside>
