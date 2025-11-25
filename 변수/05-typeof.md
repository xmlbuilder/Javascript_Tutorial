# typeof
아래는 typeof 연산자를 사용한 JavaScript 타입 확인 샘플을 설명과 함께 정리한 문서입니다.  
각 표현식이 어떤 타입을 반환하는지, 왜 그런 결과가 나오는지를 명확하게 보여드립니다.

## 📘 JavaScript typeof 타입 확인 정리
```javascript
console.log(typeof 'sample');               // "string"
console.log(typeof false);                 // "boolean"
console.log(typeof 10.5);                  // "number"
console.log(typeof ['Javascript', 'webgl']); // "object"
console.log(typeof function () {});        // "function"
console.log(typeof new Date());            // "object"
console.log(typeof /[0-9]{1,}/gi);         // "object"
console.log(typeof {a:1, b:2});            // "object"
console.log(typeof Math.exp);              // "function"
console.log(typeof null);                  // "object" ❗
console.log(typeof undefined);             // "undefined"
```


## 🧩 설명 요약표 (Markdown ASCII)
| 표현식                        | typeof 결과  | 설명                                                             |
|------------------------------|---------------|------------------------------------------------------------------|
| `'sample'`                   | `"string"`    | 문자열 리터럴                                                    |
| `false`                      | `"boolean"`   | 불리언 값                                                        |
| `10.5`                       | `"number"`    | 숫자 리터럴 (정수/실수 모두 동일)                               |
| `['Javascript', 'webgl']`    | `"object"`    | 배열은 객체로 취급됨                                             |
| `function () {}`             | `"function"`  | 함수는 특별한 `"function"` 타입으로 반환됨                       |
| `new Date()`                 | `"object"`    | Date 객체는 일반 객체로 인식됨                                   |
| `/[0-9]{1,}/gi`              | `"object"`    | 정규표현식도 객체로 취급됨                                       |
| `{a:1, b:2}`                 | `"object"`    | 일반 객체                                                        |
| `Math.exp`                   | `"function"`  | `Math.exp`는 함수이므로 `"function"`                             |
| `null`                       | `"object"`    | ❗ 버그성 설계: `null`은 객체로 인식되지만 실제 객체는 아님      |
| `undefined`                 | `"undefined"` | 값이 정의되지 않음                                               |


### ⚠️ 주의할 점
- typeof null === "object"는 JavaScript 설계 오류로 알려져 있습니다.  
    실제로는 null은 객체가 아닙니다.
- 배열, 정규표현식, 날짜 등은 모두 "object"로 반환되므로 정확한 타입 확인에는  
    Array.isArray()나 instanceof를 함께 사용하는 것이 좋습니다.
### 예:
```javascript
Array.isArray(['Javascript']) // true
new Date() instanceof Date     // true
```

---

# 4가지 타입 판별

JavaScript에서 자주 사용되는 타입 판별 방법 4가지를 비교한 문서입니다.  
각각의 특징, 반환값, 장단점, 예제까지 포함해서 정리했습니다.

## 📘 JavaScript 타입 판별 방법 비교
### ✅ 1. typeof
- 형식: typeof value
- 반환값: 문자열 ("string", "number", "object", "function" 등)
- 장점: 간단하고 빠름
- 단점: null, 배열, 정규표현식 등은 "object"로 반환됨
```javascript
typeof 'hello'           // "string"
typeof 42                // "number"
typeof null              // "object" ❗
typeof []                // "object"
typeof function() {}     // "function"
```


### ✅ 2. instanceof
- 형식: value instanceof Constructor
- 반환값: true 또는 false
- 장점: 정확한 클래스 판별 가능
- 단점: 원시값에는 사용 불가, 프로토타입 체인 기반
```javascript
[] instanceof Array           // true
new Date() instanceof Date    // true
'hello' instanceof String     // false ❗ (문자열 리터럴은 원시값)
new String('hello') instanceof String // true
```


### ✅ 3. .constructor
- 형식: value.constructor === Constructor
- 반환값: 생성자 함수
- 장점: 간단한 비교 가능
- 단점: null, undefined는 접근 시 오류 발생
```javascript
(42).constructor === Number           // true
'hi'.constructor === String           // true
[].constructor === Array              // true
({}).constructor === Object           // true
null.constructor                      // ❌ TypeError
```
- 안전하게 쓰려면 value != null && value.constructor === ... 형태로 보호 필요

### ✅ 4. Object.prototype.toString.call()
- 형식: Object.prototype.toString.call(value)
- 반환값: "[object Type]" 형식의 문자열
- 장점: 가장 정확하고 일관된 판별
- 단점: 길고 복잡함
```javascript
Object.prototype.toString.call('hi')           // "[object String]"
Object.prototype.toString.call(42)             // "[object Number]"
Object.prototype.toString.call(null)           // "[object Null]"
Object.prototype.toString.call(undefined)      // "[object Undefined]"
Object.prototype.toString.call([])             // "[object Array]"
Object.prototype.toString.call(new Date())     // "[object Date]"
Object.prototype.toString.call(/abc/)          // "[object RegExp]"
```


## 📊 비교 요약표 (Markdown ASCII)
| 방법                           | 반환값           | 정확도 | 원시값 지원 | 특징 및 주의사항                          |
|--------------------------------|------------------|--------|--------------|-------------------------------------------|
| `typeof`                      | 문자열           | 낮음   | ✅           | `null`과 배열은 `"object"`로 반환됨       |
| `instanceof`                  | true / false     | 중간   | ❌           | 원시값은 false, 프로토타입 기반           |
| `.constructor`                | 생성자 함수      | 중간   | ✅ (주의)    | `null`, `undefined`는 접근 시 오류 발생   |
| `Object.prototype.toString.call()` | "[object Type]" | 높음   | ✅           | 가장 정확, 모든 타입 구분 가능             |


## ✅ 추천 사용 전략

| 목적                        | 추천 방법                          |
|-----------------------------|-------------------------------------|
| 원시값/객체 구분            | Object.prototype.toString.call()    |
| 배열 여부 확인              | Array.isArray() 또는 .toString.call() |
| 클래스 인스턴스 확인        | instanceof                         |
| 간단한 타입 확인            | typeof                             |

---

