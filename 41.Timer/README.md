# 41장 타이머

## 41.1 호출 스케줄링

- 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용한다. 이를 호출 스케줄링이라 한다.
- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 실행할 수없다. 즉, 자바스크립트 엔진은 **싱글 스레드**로 동작한다.
- 이런 이유로 타이머 함수 setTimeout과 setInterval은 **비동기 처리 방식**으로 동작한다.

## 41.2 타이머 함수

### 41.2.1 setTimeout / clearTimeout

<aside>
📌 setTimeout 함수는 두 번째 인수로 전달받은 시간으로 단 한 번 동작하는 타이버를 생성한다. 이후 타이버가 만료되면 첫 번째 이수로 전달ㅂ다은 콜백 함수가 호출된다. 즉, setTimeout 함수의 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케줄링된다.

</aside>

```jsx
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
setTimeout(() => console.log("Hi"), 1000);

// 1초 (1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// 이때 콜백 함수에 'Lee'가 인수로 전달된다.
setTimeout((name) => console.log(`Hi ${name}`), 1000, "Lee");
```

### 41.2.2 setInterval / clearInterval

<aside>
📌 setInterval 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다. 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다. 이는 타이머가 취소될 떄까지 계속된다.

</aside>

```jsx
let count = 1;

// 1초 후 타이머가 만료될 떄마다 콜백 함수가 호출된다.
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5
  // count 5이면 setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여
  // 타이머를 취소한다. 타이머가 취소되면 setInterval 함수의 콜백 함수가 실행되지 않는다.
  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 41.3 디바운스와 스로틀

- 짧은 시간 간격으로 연속해서 발생하는 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다. 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

### 41.3.1 디바운스

- 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록한다. 즉 디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트를 핸들러가 호출되도록 한다.
- debounce 함수에 두 번째 인수로 전달한 시간보다 짧은 간격으로 이벤트가 발생하면 이전 타이버를 취소하고 새로운 타이머를 재설정한다.
- 실무에서는 underscore의 debounce함수나 Lodash의 debounce 함수를 사용하는 것을 권장한다.

### 41.3.2 스로틀

- 스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한번만 호출되도록 한다. 즉, 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- 이처럼 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 간격으로 이벤트 핸들러를 호출하는 스로틀은 scroll 이벤트 처리난 무한 스크롤 UI 구현 등에 유용하게 사용된다.
