```
╔════════════════════════════════════════════════════════════════════╗
║                   JavaScript Global 변수 동작 정리                  ║
╚════════════════════════════════════════════════════════════════════╝
```
## [GLOBAL 1] 암묵적 전역 변수 생성
────────────────────────────────────────────
```javascript
myglobal = "hello";           // 선언 없이 할당 → 전역 객체에 등록됨
console.log(myglobal);       // "hello"
console.log(window["myglobal"]); // "hello"
console.log(global["myglobal"]); // "hello"
console.log(typeof global_var);  // "undefined"
```

## [GLOBAL 2] 함수 내부에서 암묵적 전역
────────────────────────────────────────────
```javascript
function sum(x, y) {
    result = x + y;          // result는 선언 없이 사용 → 전역 등록
    return result;
}
var a = sum(1, 2);
console.log(result);         // 3 (전역 변수로 접근됨)
```

## [GLOBAL 3] var 없이 할당된 변수
────────────────────────────────────────────
```javascript
function foo() {
    var a = b = 0;           // b는 var 없이 선언 → 전역 등록됨
}
foo();
console.log(b);              // 0 (전역 변수)
```


## [GLOBAL 4] 전역 변수 선언 비교
────────────────────────────────────────────
```javascript
var global_var = 1;          // var 사용 → 삭제 불가
global_novar = 2;            // 선언 없이 할당 → 삭제 가능
global_fromfunc = function () {}; // 선언 없이 함수 할당 → 삭제 가능
```

## [GLOBAL 5] delete 연산자 동작
────────────────────────────────────────────
```javascript
console.log(delete global_var);       // false (var로 선언된 전역은 삭제 불가)
console.log(delete global_novar);     // true
console.log(delete global_fromfunc);  // true

console.log(typeof global_var);       // "number"
console.log(typeof global_novar);     // "undefined"
console.log(typeof global_fromfunc);  // "undefined"
```
- var를 생략하여 생성된 global 변수는 `Property` 로 간주 된다.

