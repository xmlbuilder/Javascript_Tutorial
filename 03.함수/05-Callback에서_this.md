# callback this

이 예제는 자바스크립트에서 콜백 함수의 this 바인딩이 어떻게 동작하는지를 아주 명확하게 보여주는 사례.  
아래에 핵심 개념과 코드 흐름을 정리.

## 🧠 콜백 함수에서의 this 특징
### 📌 핵심 개념 요약
| 개념        | 설명                                                                 |
|-------------|----------------------------------------------------------------------|
| `this`      | 함수가 **어떻게 호출되었는지** 에 따라 결정됨 (정의 시점이 아니라 호출 시점) |
| 콜백 함수   | 다른 함수의 인자로 전달되어 호출될 때, **호출 주체가 바뀌면 `this`도 바뀜** |
| `bind()`    | `this`를 **명시적으로 고정** 시켜 원하는 객체를 바인딩할 수 있음              |



### 🔍 코드 분석

```javascript
var arr = [1,2,3,4,5,6,7,8,9,10];

var obj = {
  vals: [1, 2, 3],
  logValues: function (v, i) {
    if (this.vals) {
      console.log(this.vals, v, i);
    } else {
      console.log(v, i);
    }
  }
};

arr.forEach(obj.logValues);             // ❌ this → 전역 객체 (window or undefined in strict mode)
arr.forEach(obj.logValues.bind(obj));   // ✅ this → obj
```

## 🔄 실행 흐름 설명
### 1. arr.forEach(obj.logValues);
- forEach()가 obj.logValues를 함수로서 전달받아 호출함
- 이때 this는 obj가 아니라 전역 객체가 됨
- 결과적으로 this.vals는 undefined → else 블록 실행됨
### 2. arr.forEach(obj.logValues.bind(obj));
- bind(obj)를 통해 this를 명시적으로 obj로 고정
- this.vals는 [1, 2, 3] → if 블록 실행됨
- 원하는 결과 출력됨

## 🧩 왜 이런 일이 생길까?
- 자바스크립트에서 this는 함수가 호출될 때 결정됩니다.
- 즉, obj.logValues()처럼 객체의 메서드로 호출하면 this === obj
- 하지만 forEach(obj.logValues)처럼 함수로 전달되면 호출 주체가 바뀌어 this도 바뀜

## ✅ 결론
- 콜백 함수는 호출 주체가 바뀌므로 this가 예상과 다르게 바인딩될 수 있음
- 이를 해결하려면 bind()를 사용해 명시적으로 this를 고정해야 함
- 이 개념은 `setTimeout`, `addEventListener`, `Promise`, `map`, `forEach` 등 다양한 콜백 상황에서 매우 중요함


## 핵심 개념 요약
| 메서드   | this 바인딩 | 인자 전달 방식          | 실행 시점       | 반환값         |
|----------|-------------|-------------------------|----------------|----------------|
| call     | 즉시        | 개별 인자 (쉼표로 나열) | 즉시 실행      | 함수 실행 결과 |
| apply    | 즉시        | 배열로 인자 전달        | 즉시 실행      | 함수 실행 결과 |
| bind     | 지연        | 개별 인자 (선택적)      | 나중에 실행 가능 | 새로운 함수     |


## 🔍 사용 예제 비교
```javascript
function greet(greeting, name) {
  console.log(`${greeting}, ${name}! I'm ${this.role}`);
}

const context = { role: 'Copilot' };
```
```javascript
// call
greet.call(context, 'Hello', 'JungHwan'); // Hello, JungHwan! I'm Copilot
```
```javascript
// apply
greet.apply(context, ['Hi', 'JungHwan']); // Hi, JungHwan! I'm Copilot
```
```javascript
// bind
const boundGreet = greet.bind(context, 'Hey');
boundGreet('JungHwan'); // Hey, JungHwan! I'm Copilot
```

## 🔄 차이점 정리
### ✅ call
- 즉시 실행
- 인자를 하나씩 나열
- this를 지정하고 바로 함수 실행
### ✅ apply
- 즉시 실행
- 인자를 배열로 전달
- call과 거의 동일하지만 인자 형식만 다름
### ✅ bind
- 함수를 반환 (실행은 하지 않음)
- this와 일부 인자를 고정한 새로운 함수 생성
- 지연 실행 → 나중에 호출 가능

## 📌 실전 활용 포인트

| 상황 또는 개념     | 추천 메서드         | 설명                                      |
|--------------------|---------------------|-------------------------------------------|
| `this` 즉시 바인딩 | `call`, `apply`     | 함수 실행 시 `this`를 명시적으로 지정함       |
| `this` 지연 바인딩 | `bind`              | `this`를 고정한 새 함수를 반환함              |
| 인자 배열 전달     | `apply`             | 인자를 배열로 전달할 때 유용함               |
| 재사용 가능한 함수 | `bind`              | 이벤트 핸들러나 콜백에서 `this` 고정 시 유용함 |

## 💡 요약 설명
- `call` / `apply`: 즉시 실행하면서 this를 바인딩할 때 사용
- `bind`: 나중에 실행할 함수로 this를 고정할 때 사용
- `apply`: 인자를 배열로 전달할 때 특히 유용
- `bind`: 이벤트 핸들러, 콜백 함수에서 this가 바뀌는 문제를 해결할 때 자주 사용

## ✅ 결론
- `call`과 `apply` 는 즉시 실행용
- `bind` 는 나중에 실행할 함수 생성용
- 모두 `this` 를 `명시적으로 바인딩` 할 수 있어 콜백 함수나 이벤트 핸들러에서 매우 유용

---

