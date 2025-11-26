# forEach this
이 예제는 자바스크립트에서 forEach의 **두 번째 인자(thisArg)** 가 어떻게 this를 바인딩하는지,  
그리고 함수에서 this가 어떻게 결정되는지를 아주 잘 보여주는 사례.

## ✅ 예제 코드 분석
```javascript
var arr = [1, 2, 3, 4, 5];
var entries = [];

arr.forEach(function (v, i) {
  entries.push([i, v, this[i]]);
}, [10, 20, 30, 40, 50]);

console.log(entries);
```
```
/*
[
  [0, 1, 10],
  [1, 2, 20],
  [2, 3, 30],
  [3, 4, 40],
  [4, 5, 50]
]
*/
```


## 🔍 핵심 동작 설명
- forEach(callback, thisArg)에서 두 번째 인자인 `[10, 20, 30, 40, 50]` 이 `thisArg` 로 전달됨
- 따라서 callback 함수 내부의 `this` 는 해당 배열을 참조함
- `this[i]` 는 `[10, 20, 30, 40, 50][i]` → 각각 10, 20, 30, 40, 50이 출력됨

## 📌 forEach의 this 바인딩 방식
| 요소            | 설명                                                                 |
|-----------------|----------------------------------------------------------------------|
| 첫 번째 인자     | 콜백 함수 (value, index, array)                                      |
| 두 번째 인자     | thisArg → 콜백 내부에서 사용할 `this`로 바인딩됨                      |
| 기본값           | 생략 시 `this`는 `undefined` (strict mode) 또는 `window` (non-strict) |



## 🧠 함수에서의 this 바인딩 총정리
| 함수 호출 방식           | this가 참조하는 대상                    |
|--------------------------|----------------------------------------|
| 일반 함수 호출           | 전역 객체 (`window`) 또는 `undefined` (strict) |
| 객체의 메서드 호출       | 해당 객체                              |
| 생성자 함수 (`new`) 호출 | 새로 생성된 인스턴스 객체              |
| `call`, `apply` 호출     | 명시적으로 지정한 객체                 |
| `bind` 호출              | 고정된 객체로 바인딩된 새 함수         |
| `forEach`의 thisArg      | 두 번째 인자로 전달된 객체             |
| 화살표 함수              | 상위 스코프의 `this`를 그대로 사용함     |

## 💡 실전 팁
- forEach에서 thisArg를 활용하면 외부 데이터를 콜백 내부에서 쉽게 참조할 수 있음
- this는 정의 시점이 아니라 호출 시점에 결정되므로, 콜백이나 비동기 함수에서는 bind 또는 화살표 함수를 활용하는 것이 안전함

## ✅ 결론
- forEach의 두 번째 인자는 thisArg로서 콜백 내부의 this를 바인딩하는 역할을 함
- 함수에서의 this는 호출 방식에 따라 달라지므로, 상황에 맞게 bind, call, apply, 또는 화살표 함수를 활용해야 함

---
# map, filter, setTimeout, addEventListener

이번엔 자바스크립트에서 자주 쓰이는 함수들 — map, filter, setTimeout, addEventListener — 에서의 this 바인딩 방식에 대해 확장해서 정리.

## 🧠 함수별 this 바인딩 방식 요약
| 함수명           | 기본 `this` 바인딩 방식                  | 해결 방법 또는 특징                          |
|------------------|------------------------------------------|----------------------------------------------|
| `map`, `filter`  | 두 번째 인자로 `thisArg` 전달 가능       | `forEach`와 동일하게 `thisArg`로 바인딩 가능 |
| `setTimeout`     | 일반 함수 사용 시 `this`는 전역 객체     | 화살표 함수 또는 `bind()`로 해결             |
| `addEventListener` | 콜백 내부의 `this`는 이벤트 대상 요소   | `this`는 이벤트를 받은 DOM 요소를 가리킴     |



## 🔍 상세 설명 + 예제
### ✅ map, filter — thisArg로 바인딩
```javascript
const arr = [1, 2, 3];
const context = { factor: 10 };

const result = arr.map(function(v) {
  return v * this.factor;
}, context);

console.log(result); // [10, 20, 30]
```

- thisArg로 context를 전달하면 콜백 내부에서 this.factor 사용 가능

## ✅ setTimeout — 전역 this 문제
```javascript
const obj = {
  name: 'JungHwan',
  sayHi: function() {
    setTimeout(function() {
      console.log(`Hi, I'm ${this.name}`);
    }, 1000);
  }
};
obj.sayHi(); // Hi, I'm undefined (전역 this)
```
```javascript
const fixedObj = {
  name: 'JungHwan',
  sayHi: function() {
    setTimeout(() => {
      console.log(`Hi, I'm ${this.name}`);
    }, 1000);
  }
};

fixedObj.sayHi(); // Hi, I'm JungHwan (화살표 함수로 해결)
```
- 일반 함수는 this가 전역 객체
- 화살표 함수는 상위 스코프의 this를 유지

## ✅ addEventListener — this는 이벤트 대상
```html
<button id="btn">Click me</button>
```
```javascript
document.getElementById('btn').addEventListener('click', function() {
  console.log(this); // <button> 요소
});
```

- this는 이벤트를 받은 DOM 요소
- 화살표 함수 사용 시 this는 상위 스코프로 바뀌므로 주의

## 📌 정리 요약
| 상황                     | `this` 바인딩 방식                  | 해결 방법                     |
|--------------------------|-------------------------------------|-------------------------------|
| 배열 메서드 (`map`, `filter`) | `thisArg`로 명시적 바인딩 가능       | 두 번째 인자 사용              |
| `setTimeout`             | 전역 객체 또는 `undefined`          | 화살표 함수 / `bind()`         |
| `addEventListener`       | 이벤트 대상 DOM 요소                | 일반 함수 사용 (화살표 함수 주의) |

- 👉 이처럼 this는 호출 방식과 함수 종류에 따라 다르게 바인딩되므로, 상황에 맞는 전략 (thisArg, bind, 화살표 함수 등)을 선택하는 것이 중요합니다.

---



