# String

아래는 JavaScript String 객체의 주요 메서드와 특히 헷갈리기 쉬운 `substring()` 과 `slice()` 의 차이를 정리한 문서입니다.

## 📘 JavaScript String 함수 정리
### ✅ 주요 메서드 예시
```javascript
var str = "String Test";
console.log(str.toLowerCase()); // "string test"
console.log(str.toUpperCase()); // "STRING TEST"
```
```javascript
var str2 = "Sample Model Sample Model";
console.log(str2.indexOf("Sa"));      // 0
console.log(str2.lastIndexOf("Sa"));  // 13
console.log(str2.indexOf("Mo", 10));  // 20
console.log(str2.indexOf("model"));   // -1 (대소문자 구분)
```
```javascript
var sample1 = "강아지";
var sample2 = "고양이 2마리";
console.log(sample1.length); // 3
console.log(sample2.length); // 7
```
```javascript
var str3 = "프로그램 강의에 오신것을 환영합니다";
console.log(str3.substring(0, 4)); // "프로그램"
console.log(str3.substring(5));    // "강의에 오신것을 환영합니다"
console.log(str3.substring(14, 19)); // "환영합니다"
console.log(str3.slice(0, 4));     // "프로그램"
console.log(str3.slice(14, 19));   // "환영합니다"
console.log(str3.charAt(14));      // "환"
```


## 📊 String 메서드 요약표 (Markdown ASCII)
| 메서드              | 설명                                                                 | 예시 결과                          |
|---------------------|----------------------------------------------------------------------|------------------------------------|
| toLowerCase()       | 문자열을 모두 소문자로 변환                                          | "String Test" → "string test"      |
| toUpperCase()       | 문자열을 모두 대문자로 변환                                          | "String Test" → "STRING TEST"      |
| indexOf(substr)     | 지정한 문자열의 첫 번째 위치 반환 (없으면 -1)                        | "Sample..."에서 "Sa" → 0           |
| lastIndexOf(substr) | 지정한 문자열의 마지막 위치 반환                                    | "Sample..."에서 "Sa" → 13          |
| length              | 문자열 길이 반환 (유니코드 문자 단위)                               | "강아지" → 3, "고양이 2마리" → 7   |
| substring(start,end)| start~end 사이 문자열 반환 (end는 제외, 음수는 0으로 처리)           | substring(14,19) → "환영합니다"    |
| slice(start,end)    | start~end 사이 문자열 반환 (end는 제외, 음수 인덱스 허용)            | slice(14,19) → "환영합니다"        |
| charAt(index)       | 지정한 위치의 문자 반환                                             | charAt(14) → "환"                  |


## ✅ substring() vs slice() 차이

| 구분            | substring() 동작                          | slice() 동작            |
|-----------------|-------------------------------------------|-------------------------|
| 음수 인덱스     | 음수는 0으로 처리                         | 음수는 뒤에서부터 계산  |
| 인자 순서       | start > end → 자동으로 교환               | start > end → "" 반환   |
| 사용 예시       | substring(5, 2) → substring(2, 5) 결과 동일 | slice(5, 2) → ""        |

- `substring()` 은 음수를 0으로 처리하고, 인자 순서를 자동 교환합니다.
- `slice()` 는 음수 인덱스를 허용하며, 인자 순서를 교환하지 않고 빈 문자열을 반환합니다.

## ✅ 핵심 포인트
- 대소문자 변환: toLowerCase(), toUpperCase()
- 검색: indexOf(), lastIndexOf()
- 길이: .length
- 부분 문자열 추출: substring()과 slice()
- substring()은 음수를 0으로 처리, 인자 순서 자동 교환
- slice()는 음수 인덱스 허용, 인자 순서 교환 없음
- 문자 추출: charAt(index)

---

# substring() /  slice() / substr()
아래는 substring(), slice(), substr() 세 가지 문자열 추출 함수의 비교표와 대소문자 구별하지 않고  
문자열 검색하는 방법까지 정리한 문서입니다.


## ✅ 문자열 추출 함수 비교

| 함수        | 특징                                                   | 음수 인덱스 처리        | 인자 순서 처리            | 사용 예시                          |
|-------------|--------------------------------------------------------|-------------------------|---------------------------|------------------------------------|
| substring() | start ~ end 사이 문자열 반환 (end 제외)                | 음수는 0으로 처리       | start > end → 자동 교환   | substring(5, 2) → substring(2, 5) |
| slice()     | start ~ end 사이 문자열 반환 (end 제외)                | 음수 허용 (뒤에서 계산) | start > end → "" 반환     | slice(-5, -1) → 끝에서 4글자 추출 |
| substr()    | start 위치부터 length 길이만큼 문자열 반환             | 음수 허용 (뒤에서 계산) | length만큼 반환           | substr(5, 3) → 5번째부터 3글자    |



## ✅ 대소문자 구별하지 않는 방법
JavaScript의 문자열 검색(indexOf, includes, match)은 기본적으로 대소문자를 구분합니다.  
대소문자를 무시하려면 검색 대상과 검색어를 모두 동일한 대소문자로 변환한 뒤 비교합니다.
### 🔹 예시 1: toLowerCase() 활용
```javscript
var str = "Hello World";
console.log(str.toLowerCase().indexOf("hello")); // 0
console.log(str.toLowerCase().includes("world")); // true
```

### 🔹 예시 2: 정규표현식 + i 플래그
```javscript
var str = "Hello World";
console.log(/hello/i.test(str));   // true
console.log(str.match(/world/i));  // ["World"]
```


## 📌 핵심 요약
- substring() → 음수는 0 처리, 인자 순서 자동 교환
- slice() → 음수 인덱스 허용, 인자 순서 교환 없음
- substr() → 시작 위치부터 지정한 길이만큼 추출
- 대소문자 무시 검색 → toLowerCase() / toUpperCase() 변환 또는 정규표현식 i 플래그 사용

---

