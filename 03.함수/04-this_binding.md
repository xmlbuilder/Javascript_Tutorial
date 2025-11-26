# this binding

이 코드는 자바스크립트에서 객체의 메서드 호출 시 this 바인딩이 어떻게 작동하는지를 아주 잘 보여주는 예제. 

## 🧠 코드 분석
```javascript
var obj = {
  a: 1,
  b: function () {
    console.log(this);
  },
  c: function () {
    console.log(this.a);
  }
};

obj.b();         // { a: 1, b: [Function: b], c: [Function: c] }
obj.c();         // 1
console.dir(obj.b); // [Function: b]
console.dir(obj.c); // [Function: c]
```
## 🔍 실행 결과 설명
### ✅ obj.b();
- this는 **메서드를 호출한 객체 obj** 를 가리킴
- 따라서 console.log(this)는 obj 전체를 출력
- 출력: `{ a: 1, b: [Function: b], c: [Function: c] }`
### ✅ obj.c();
- this.a는 obj.a를 의미
- 출력: 1
### ✅ console.dir(obj.b);
- obj.b는 함수 자체를 참조
- 출력: [Function: b]
## ✅ console.dir(obj.c);
- obj.c도 함수 자체를 참조
- 출력: [Function: c]


## 📌 핵심 개념 요약

| 개념           | 설명                            |
|----------------|---------------------------------|
| `this`         | `obj.b()` 호출 시 `this === obj` |
| `this.a`       | `obj.a` 참조 → `1` 출력           |
| `console.dir()`| 함수 자체를 구조적으로 출력       |


## 🧠 실행 컨텍스트 흐름 요약
1. 전역 실행 컨텍스트 생성
2. 변수 `obj` 선언 → 객체 생성 및 주소 할당
3. `obj.b()` 호출 → `this`는 `obj` → 전체 객체 출력
4. `obj.c()` 호출 → `this.a`는 `obj.a` → `1` 출력
5. `console.dir(obj.b)` → 함수 자체 출력
6. `console.dir(obj.c)` → 함수 자체 출력



## ✅ 결론
이 코드는 자바스크립트에서 객체 메서드 호출 시 this가 어떻게 바인딩되는지,
그리고 함수 자체를 참조할 때와 실행할 때의 차이를 명확하게 보여줍니다.


---

