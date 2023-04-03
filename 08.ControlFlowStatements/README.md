# 8장 제어문

<aside>
📌 제어문은 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다. 일반적으로 코드는 위에서 아래 방향으로 순차적으로 실행된다. 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

</aside>

- 제어문은 코드의 흐름을 이해하기 어렵게 만들어 가독성을 해치는 단점이 있다. 가독성이 좋지 않은 코든느 오류를 발생시키는 원인이 된다.
- 제어문을 바르게 이해하는 것은 코딩 스킬에 많은 영항을 준다 특히 for문은 매우 중요하므로 확실히 이해하자.

## 8.1 블록문

<aside>
📌 블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다. 블록문은 단독으로 사용할 수도 있으나 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

</aside>

다음은 블록문이 사용되는 다양한 예제다.

```jsx
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

- 문의 끝에는 세미클론 붙이는 것이 일반적이나, 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖는다.
- 때문에 끝에는 세미콜론을 붙이지 않는다.

## 8.2 조건문

<aside>
📌 조건문은 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다. 조건식은 불리언 값으로 평가될 수 있는 표현식이다.

</aside>

### 8.2.1 if …else문

```jsx
if (조건식1) {
  // 조건식1이 참이면 이 코드 블록이 실행된다.
} else if (조건식2) {
  // 조건식2가 참이면 이 코드 블록이 실행된다.
} else {
  // 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}
```

```jsx
// 삼항연산자
// 조건식이 참이면 결과1 아니면 결과2
var result = 조건식1 ? "결과1" : "결과2";
```

### 8.2.2 switch문

<aside>
📌 switch문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.

</aside>

- switch문의 표현식과 일치하는 case문이 없다면 실행 순서는 default 문으로 이동한다. default문은 선택사항으로, 사용할 수도 있고 사용하지 않을 수도 있다.

```jsx
var month = 11;
var monthName;

switch (month) {
  case 1:
    monthName = "Jan";
  case 2:
    monthName = "Feb";
  case 3:
    monthName = "Mar";
  case 4:
    monthName = "Apr";
  case 5:
    monthName = "May";
  case 6:
    monthName = "June";
  case 7:
    monthName = "July";
  case 8:
    monthName = "Aug";
  case 9:
    monthName = "Sep";
  case 10:
    monthName = "Oct";
  case 11:
    monthName = "Nov";
  case 12:
    monthName = "Dec";
  default:
    monthName = "Invalid month";
}

console.log(monthName); // Invalid month
```

- 위 예제를 실행해 보면 “Nov”가 출력되지 않고 “Invalid month” 가 출력된다.
- 맞는 문을 실행한 후 switch문을 탈출하지 않고 switch문이 끝날 때까지 이후의 모든 case문과 default 문을 실행했기 때문이다. 이를 **폴 스루**라 한다.

```jsx
var month = 11;
var monthName;

switch (month) {
	case 1: monthName = "Jan";
		break;
	case 2: monthName = "Feb";
		break;
	case 3: monthName = "Mar";
		break;
	case 4: monthName = "Apr";
		break;
	case 5: monthName = "May";
		break;
			.
			.
			.
	case 12: monthName = "Dec";
		break;
	default: monthName = "Invalid month";
}
console.log(monthName); // Nov
```

## 8.3 반복문

<aside>
📌 반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다.

</aside>

### 8.3.1 for문 동작 순서

```jsx
for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

- for 문을 실행하면 맨 먼저 변수 선언문 var i = 0이 실행된다 변수 선언문은 단 한 번만 실행된다.
- 변수 선언문의 실행이 종료되면 조건식이 실행된다. 현재 i 변수의 값은 0이므로 조건식의 평가 결과는 true다.
- 조건식의 평가 결과가 true이므로 코드 블록이 실행된다. 증감문으로 실행 흐름이 이동하는 것이 아니라 코드 블록으로 실행 흐름이 이동하는 것에 주의하자.
- 코드 블록의 실행이 종료되면 증감식 i++가 실행되어 i 변수의 값은 1이 된다.
- 증감식 실행이 종료 되면 다시 조건식이 실행된다.
- 조건식 평가 결과가 true이므로 코드 블록이 다시 실행된다.
- 코드 블록의 실행이 종료되면 증감식 i++가 실행되어 i 변수의 값은 2가된다.
- 증감식 실행이 종료되면 다시 실행된다 조건식의 평가 결과가 false이면 실행이 종료된다.

### 8.3.2 while문

<aside>
📌 while문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.

</aside>

- for문은 반복 횟수가 명확할 때 주로 사용하고 while문은 반복 횟수가 불명확할 때 주로 사용한다.

```jsx
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```

## 8.4 break 문

<aside>
📌 break 문은 코드 블록을 탈출한다.

</aside>

- 좀 더 정확히 표현하자면 코드블록을 탈출하는 것이 아니라 레이블 문, 반복문, switch문의 코드블록을 탈출한다.
- 이 외에 break문을 사용하면 SyntaxError가 발생한다.

## 8.5 continue 문

<aside>
📌 continue 문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. break 문처럼 반복문을 탈출하지는 않는다.

</aside>
```jsx
var string = 'Hello World';
var search = 'l';
var count = 0;

// 문자열은 유사 배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
// 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
if (string[i] !== search) continue;
count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}

console.log(count); // 3

```

```
