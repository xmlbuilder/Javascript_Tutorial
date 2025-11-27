
# 📘 JavaScript에서 eval과 스코프(Scope)의 관계 정리
## 1. 객체 속성 접근
```javascript
var obj = { name: "test" };
var property = "name";
console.log("obj." + property);     // 문자열: "obj.name"
console.log(obj[property]);         // 실제 값: "test"
```
- obj[property]는 동적 속성 접근 방식으로 "test"를 출력한다.

## 2. eval vs Function의 스코프 차이
변수 선언 전 typeof 검사
```javascript
console.log(typeof un);    // "undefined"
console.log(typeof deux);  // "undefined"
console.log(typeof trois); // "undefined"
```
- 아직 선언되지 않은 변수는 typeof 검사 시 "undefined"를 반환한다.

### eval을 통한 변수 선언
```javascript
eval("var un = 1; console.log(un);"); // 출력: 1
```
- eval은 현재 스코프에서 실행되므로 un은 전역 변수로 등록된다.

### Function을 통한 변수 선언
```javascript
new Function("var deux = 2; console.log(deux);")(); // 출력: 2
```
- Function은 새로운 함수 스코프를 생성하므로 deux는 외부에서 접근 불가.

### 다시 eval로 변수 선언
```javascript
eval("var trois = 3; console.log(trois);"); // 출력: 3
```javascript
- trois도 eval을 통해 전역 변수로 등록됨.

### 이후 typeof 검사
```javascript
console.log(typeof un);    // "number"
console.log(typeof deux);  // "undefined"
console.log(typeof trois); // "number"
```
- un, trois는 전역 스코프에 존재 → "number"
- deux는 Function 내부에만 존재 → "undefined"

## 3. 함수 내부에서의 eval vs Function
### eval은 로컬 스코프에 접근 가능
```javascript
(function () {
  var local = 1;
  eval("local = 2;");
  console.log(local); // 출력: 2
})();
```
- eval은 함수 내부 로컬 변수에 접근하고 수정 가능.

### Function은 로컬 스코프에 접근 불가
```javascript
(function () {
  var local = 1;
  Function("console.log(typeof local);")(); // 출력: "undefined"
})();
```
- Function은 자신만의 스코프를 가지므로 외부 변수 local에 접근 불가.

## ✅ 핵심 요약: JavaScript에서 `eval` vs `Function` 스코프 비교

| 항목             | `eval`                                                  | `Function`                                              |
|------------------|----------------------------------------------------------|----------------------------------------------------------|
| 실행 스코프       | 현재 스코프에서 실행됨                                     | 새로운 함수 스코프 생성                                   |
| 변수 접근         | 로컬 및 전역 변수 접근 가능                                 | 외부 변수 접근 불가                                       |
| 변수 선언         | 현재 스코프에 변수 등록됨                                  | 내부 스코프에만 변수 등록됨                              |
| `typeof` 결과     | 선언된 변수는 `"number"` 등으로 인식됨                     | 외부 변수는 `"undefined"`로 인식됨                        |
| 보안 및 성능      | 느리고 보안상 위험할 수 있음 (`eval`은 코드 삽입 가능성 있음) | 상대적으로 안전하지만 외부 스코프 접근이 제한됨           |
| 사용 목적         | 동적 코드 실행, 디버깅, 실험적 로직 등                      | 안전한 동적 함수 생성, 외부 스코프와 분리된 실행 환경 필요 시 |

---



👉 이 문서는 eval과 Function의 스코프 차이를 명확히 보여주는 실험 코드 기반 분석입니다. 원하시면 이 내용을 PDF나 Markdown 문서로 포맷팅해 드릴 수도 있어요!
