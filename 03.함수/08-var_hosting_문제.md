# var hoisting and closure behavior
아래 샘플 코드를 기반으로, 왜 var로 “10이 10번” 출력되는 문제가 생기는지와 이를 해결하는 방법을 단계적으로 설명합니다.  
핵심은 “호출 시점의 값”과 “스코프(변수 바인딩)가 어디에 존재하느냐”입니다.

## Why the var hoisting problem happens
- 호이스트의 의미:
- 자바스크립트는 스코프(함수 또는 전역)의 최상단에서 변수 선언을 미리 처리합니다.  
- var i는 **선언** 이 스코프 상단으로 끌어올려지고, 초기화는 원래 위치에서 진행됩니다.
- 단일 바인딩:
- for (var i=0; i<10; i++) { ... }는 스코프에 대해 오직 하나의 i 바인딩을 사용합니다.  
  - 반복마다 같은 변수 i가 재할당될 뿐, 새로 만들어지지 않습니다.
- 클로저의 참조 특성:
  - 루프 안에서 만들어진 함수들은 **당시의 값** 을 복사하는 것이 아니라, **같은 i 변수(단일 바인딩)를 참조** 합니다.
  - 루프 종료 후 i는 10이 되고, 나중에 f()를 호출할 때 모두 같은 i(10) 을 읽습니다.
### 샘플 1: var로 인해 모두 10
```javascript
var funcs = [];
for (var i = 0; i < 10; i++) {
  funcs.push(function () {
    console.log(i);
  });
}
funcs.forEach(function (f) { f(); });
// 출력: 10 10 10 10 10 10 10 10 10 10
```

- 핵심: i는 하나뿐이고, 최종 값 10을 가리킵니다. 각 콜백은 호출 시점에 같은 i를 참조합니다.

### Solution 1: IIFE로 값 캡처(별도 바인딩 만들기)
- 아이디어:
- 즉시 실행 함수(IIFE)에 현재 i 값을 인자로 넘겨, 내부에서 새로운 지역 변수 v 에 담아 `클로저가 그 v를 참조` 하게 만듭니다.
- 효과:
  - 반복마다 서로 다른 바인딩(v) 이 생기므로, 콜백은 해당 반복의 값을 정확히 출력합니다.
### 샘플 2: IIFE로 올바르게 0~9
```javascript
var funcs = [];
for (var i = 0; i < 10; i++) {
  funcs.push((function (v) {
    return function () {
      console.log(v);
    };
  })(i));
}
funcs.forEach(function (f) { f(); });
// 출력: 0 1 2 3 4 5 6 7 8 9
```

- 핵심: 각 콜백은 자신의 스코프에 있는 v(반복 시점의 i)를 캡처합니다.

### Solution 2: let으로 반복마다 새 바인딩 생성
- 블록 스코프:
  - let은 블록 스코프를 갖습니다. for (let i=...)는 반복마다 새로운 i 바인딩을 만듭니다.
- 콜백은 생성 시점의 해당 반복 바인딩을 캡처하고, 나중에 호출해도 그 값을 유지합니다.
- TDZ(Temporal Dead Zone):
- let도 선언은 호이스팅되지만, 초기화 전 접근은 TDZ로 인해 에러가 나므로 var처럼 **초기화 전 undefined 접근** 이 발생하지 않습니다. 
  - 이 특성 덕분에 예측 가능한 스코프 동작을 보장합니다.
### 샘플 3: let으로 올바르게 0~9
```javascript
var funcs = [];
for (let i = 0; i < 10; i++) {
  funcs.push(function () {
    console.log(i);
  });
}
funcs.forEach(function (f) { f(); });
// 출력: 0 1 2 3 4 5 6 7 8 9
```

- 핵심: 반복마다 i가 새로 만들어지므로, 각 콜백이 해당 반복의 i를 캡처합니다.

## What “context execution assigns values” really means
- 실행 컨텍스트에서의 평가:
- 함수 본문에 있는 console.log(i)는 호출 시점에 i를 조회합니다.
- var 루프에서는 단일 바인딩의 최신값(10)을 읽고, IIFE/let 해결책에서는 각기 다른 바인딩(반복별 값)을 읽습니다.
- 요약:
- **값이 대입된다** 기보단, 같은 식별자 i가 어떤 바인딩을 참조하느냐가 결과를 결정합니다.

## Additional practical solutions
- 인덱스를 인자로 넘기기:
  - funcs.push(((idx) => () => console.log(idx))(i)); → IIFE를 간결히 화살표 함수로 표현
- 배열 유틸을 활용:
  - Array.from({length:10}, (_, i) => () => console.log(i)) → 반복마다 독립 바인딩
- 콜백에 파라미터로 값 주입:
- 값을 나중에 사용할 함수에 명시적으로 인자로 전달해 클로저 의존성을 줄이기

## Quick recap
- var 문제 원인:
  - 스코프에 하나의 i만 존재(호이스팅 + 함수/전역 스코프), `콜백은 호출 시점` 에 그 단일 바인딩을 읽음 → 모두 10.
- 해결책:
  - IIFE로 별도 바인딩 생성 또는 let으로 반복마다 새 바인딩.
- 권장:
  - ES6+ 환경에서는 let을 기본으로, 필요 시 IIFE/매개변수 주입을 보완적으로 사용.

---

