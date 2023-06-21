# 37장 Set과 Map

## 37.1 Set

- **Set 객체는 중복되지 않는 유일한 값들의 집합이다**. Set 객체는 **배열과 유사**하지만 다음과 같은 차이가 있다.

| 구분                                 | 배열 | Set 객체 |
| ------------------------------------ | ---- | -------- |
| 동일한 값을 중복하여 포함할 수 있다. | O    | X        |
| 요소 순서에 의미가 있다.             | O    | X        |
| 인덱스로 요소에 접근할 수 있다.      | O    | X        |

### 37.1.1 Set 객체의 생성

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

- **Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.**

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set("Hello");
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

### 37.1.2 요소 개수 확인

- Set 객체의 요소 개수를 확인할 떄는 Set.prototype.size 프로퍼티를 사용한다.

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 37.1.3 요소 추가

- Set 객체에 요소를 추가할 떄는 Set.prototype.add 메서드를 사용한다.

```jsx
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

- add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. 따라서 add 메서드를 호출한 후에 add 메서드를 연속적으로 호출할 수 있다.

```jsx
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2), {1, 2}
```

- Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

### 37.1.4 요소 존재 여부 확인

- Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다. has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 37.1.5 요소 삭제

- Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다. delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

```jsx
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

- 만약 존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시된다.
- delete 메서드는 삭제 성공여부를 나타내는 불리언 값으로 반환한다 따라서 연속적으로 호출할 수 없다.

### 37.1.6 요소 일괄 삭제

- Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서들르 사용한다. clear 메서드는 언제나 undefined를 반환한다

```jsx
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### 37.1.7 요소 순회

- Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다
  - 첫 번째 인수 : 현재 순회 중인 요소값
  - 두 번째 인수 : 현재 순회 중인 요소값
  - 세 번째 인수 : 현재 순회 중인 Set 객체 자체

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));

/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- **Set 객체는 이터러블이다**. 따라서 for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

### 37.1.8 집합 연산

<aside>
📌 Set 객체는 수학적 집합을 구현하기 위한 자료구조다. 따라서 Set객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다.

</aside>

1. **교집합**
   - A와 집합 B의 공통 요소로 구성된다.
2. **합집합**
   - 합집합은 집합 A와 집합 B의 중복 없는 모든 요소로 구성된다.
3. **차집합**
   - 차집합은 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성된다.

4**.부분 집합과 상위 집합**

- 집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분 집합이며, 집합 B는 집합 A의 상위 집합이다.

## 37.2 Map

- **Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다**. Map 객체는 **객체와 유사** 하지만 다음과 같은 차이가 있다.

| 구분                   | 객체                    | Map 객체             |
| ---------------------- | ----------------------- | -------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값     | 객체를 포함한 모든값 |
| 이터러블               | X                       | O                    |
| 요소 개수 확인         | Object.keys(obj).length | map.size             |

### 37.2.1 Map 객체의 생성

- Map 객체는 Map 생성자 함수로 생성한다. Map 생성자 함수에 인수를 전달하지 않으면 빈 Map 객체가 생성된다.

```jsx
const map = new Map();
console.log(map); // Map(0) {}
```

- **Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.**

```jsx
const map1 = new Map([
  ["key1", "value1"],
  ["key1", "value2"],
]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError
```

- Map 객체에는 중복된 키를 갖는 요소거 존재할 수 없다.

### 37.2.2 요소 개수 확인

- Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.

```jsx
const { size } = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(size); // 2
```

### 37.2.3 요소 추가

- Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.

```jsx
const map = new Map();
cosole.log(map); // Map(0) {}

map.set("key1", "value1");
console.log(map); // Map(1) {"key1" => "value1"}
```

- Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 떄문에 중복된 키를 갖는 요소를 추가하면 값이 덮어 써진다.

### 37.2.4 요소 취득

- Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다. get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다. 요소가 존재 하지않으면 undefined를 반환한다.

### 37.2.5 요소 존재 여부 확인

- Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용한다. has 메서드는 특정 요소 존재 여부를 나타내는 불리언 값을 반환한다.

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "denveloper"],
  [kim, "designer"],
]);

console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

### 37.2.6 요소 삭제

- Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.

### 37.2.7 요소 일괄 삭제

- Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.

### 37.2.8 요소 순회

- Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.
  - 첫 번째 인수 : 현재 순회 중인 요소값
  - 두 번째 인수 : 현재 순회 중인 요소키
  - 세 번째 인수 : 현재 순회 중인 Map 객체 자체

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.forEach((v, k, map) => console.log(v, k, map));

/*
developer {name: 'Lee'} Map(2) {
	{name: "Lee"} => "developer",
	{name: "kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
	{name: "Lee"} => "developer",
	{name: "Kim"} => "designer"
}
*/
```

- Map 객체는 이터러블이다. 따라서 for… of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭 처링 할당의 대상이 될 수도 있다.
- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.
