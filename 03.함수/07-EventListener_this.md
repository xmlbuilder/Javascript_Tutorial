# EventListener this
addEventListener에서 this가 버튼 요소를 가리키게 되는 과정은 실행 컨텍스트와 이벤트 시스템의 바인딩 규칙에 따라 자동으로 처리됩니다.  
아래에 그 흐름을 코드와 함께 단계별로 설명.

## ✅ 예제 코드
```html
<button id="btn">Click me</button>
```
```javascript
document.getElementById('btn').addEventListener('click', function () {
  console.log(this);           // <button> 요소
  console.log(this.id);        // "btn"
  this.style.backgroundColor = 'yellow'; // 버튼 배경색 변경
});
```


## 🔍 this가 버튼으로 바뀌는 과정
### 1. addEventListener 등록
```javascript
element.addEventListener('click', callback);
```
- element는 이벤트를 받을 DOM 요소 (<button> 등)
- callback은 이벤트 발생 시 실행될 함수
### 2. 이벤트 발생 → 브라우저가 콜백 호출
- 브라우저는 이벤트가 발생하면 내부적으로 callback.call(element)처럼 호출
- 즉, 콜백 함수의 this를 이벤트 대상 요소로 바인딩해서 실행
### 3. 실행 컨텍스트 생성
- 콜백 함수가 실행되면서 새로운 실행 컨텍스트가 생성됨
- 이때 this는 이벤트를 받은 DOM 요소로 자동 설정됨

## 🧠 내부적으로 일어나는 일 (의사 코드)
```javascript
const btn = document.getElementById('btn');

function handler() {
  console.log(this); // this === btn
}

btn.addEventListener('click', handler);

// 브라우저 내부적으로는 이렇게 실행됨:
handler.call(btn);
```

### ⚠️ 화살표 함수 사용 시 주의
```javascript
btn.addEventListener('click', () => {
  console.log(this); // ❌ this는 상위 스코프 (예: window)
});
```

- 화살표 함수는 자신의 this를 가지지 않고 상위 스코프의 this를 그대로 사용
- 따라서 DOM 요소를 가리키지 않음 → 일반 함수 사용해야 함

## ✅ 결론
- addEventListener는 이벤트 발생 시 콜백 함수의 this를 이벤트 대상 요소로 자동 바인딩
- 이는 브라우저가 내부적으로 callback.call(element)처럼 처리하기 때문
- 화살표 함수는 this 바인딩을 하지 않으므로 일반 함수 사용이 필수

---

# delegate / removeEventListener

이번엔 자바스크립트에서의 **이벤트 위임(delegate)** 과 removeEventListener()에서의 this 처리 방식까지 확장해서 설명.  
이 두 개념은 이벤트 시스템을 효율적으로 다루는 데 핵심적인 역할을 합니다.

## ✅ 이벤트 위임(delegate)에서의 this
### 📌 개념
- 이벤트 위임은 하위 요소마다 이벤트를 등록하지 않고, 상위 요소에 이벤트를 등록한 뒤 이벤트 버블링을 활용하는 방식입니다.
- this는 이벤트를 등록한 요소를 가리킵니다.
- 실제 클릭된 요소는 `event.target` 으로 접근합니다.
### 🧠 예제
```html
<ul id="menu">
  <li>Home</li>
  <li>About</li>
  <li>Contact</li>
</ul>
```
```javascript
document.getElementById('menu').addEventListener('click', function (event) {
  console.log('this:', this);           // <ul id="menu">
  console.log('event.target:', event.target); // 클릭된 <li> 요소
});
```

#### 🔍 설명
- this는 이벤트를 등록한 `#menu` 요소
- event.target은 실제 클릭된 `<li>` 요소
- 이렇게 하면 동적으로 생성된 하위 요소에도 이벤트를 적용할 수 있어 효율적

## ✅ removeEventListener에서의 this
### 📌 개념
- removeEventListener는 addEventListener와 같은 함수 참조를 전달해야만 이벤트를 제거할 수 있음
- this는 이벤트를 제거하는 대상 요소를 가리킴

### 🧠 예제
```javascrpt
function handleClick() {
  console.log('Clicked:', this.id);
}

const btn = document.getElementById('btn');
btn.addEventListener('click', handleClick);

// 나중에 제거할 때
btn.removeEventListener('click', handleClick); // ✅ 정확히 같은 함수 참조여야 함
```


### ⚠️ 주의: 익명 함수는 제거 불가
```javascript
btn.addEventListener('click', function () {
  console.log('clicked');
});

btn.removeEventListener('click', function () {
  console.log('clicked');
}); // ❌ 작동하지 않음 → 서로 다른 함수 참조

```

## 📌 정리 요약

| 상황 또는 함수             | `this` 바인딩 대상           | 실제 클릭된 요소 (`event.target`) |
|----------------------------|------------------------------|-----------------------------------|
| `addEventListener` 일반 함수 | 이벤트 등록한 DOM 요소       | 이벤트가 발생한 하위 요소         |
| 이벤트 위임(delegate)       | 상위 요소 (예: `<ul>`)       | 하위 요소 (예: `<li>`)            |
| 화살표 함수 사용 시         | 상위 스코프의 `this`         | `event.target`은 그대로 사용 가능 |
| `removeEventListener`       | 이벤트 제거 대상 요소        | 해당 없음                         |


- 👉 이처럼 this는 이벤트 시스템에서 등록 주체와 실행 컨텍스트에 따라 달라지며,  
  `event.target`, `bind`, `arrow function`, `named function` 등을 적절히 활용해야 원하는 동작을 얻을 수 있습니다.

---
