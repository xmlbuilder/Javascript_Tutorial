## static 패턴
지금 예제는 JavaScript에서 **static 변수** 처럼 동작하는 패턴을 보여주는 코드입니다.  
아래에 개념과 동작 방식을 정리.

## 📘 JavaScript에서의 **static 변수** 란?
JavaScript에는 클래스 기반 언어처럼 static 키워드가 명시적으로 존재하지 않지만,  
함수나 객체의 속성으로 직접 할당하면 static처럼 동작합니다.

## 🔍 예제 분석
```javascript
var Util = function() {};             // 생성자 함수 정의
Util.VERSION = "1.0";                // static 변수처럼 동작하는 속성
Util.getBmi = function(weight, height) {
    return weight / (height * height); // static 메서드처럼 동작
};

console.log(Util.VERSION);           // "1.0"
console.log(Util.getBmi(81, 1.76));  // 26.14...
```


## ✅ 특징
- VERSION과 getBmi는 Util 함수 자체의 속성입니다.
- new Util()로 만든 인스턴스에는 포함되지 않습니다.
- 즉, 클래스의 static 필드/메서드처럼 공유되는 속성입니다.

## 🧩 객체 vs 인스턴스 비교
```java
var u = new Util();
console.log(u.VERSION); // undefined
console.log(Util.VERSION); // "1.0"
```
- VERSION은 인스턴스가 아니라 Util 자체에 붙어 있으므로 u.VERSION은 undefined

## ✅ 클래스 문법에서의 static (ES6)
- ES6 이후에는 class 문법에서 static 키워드를 사용할 수 있습니다:
```javascript
class Util {
    static VERSION = "1.0";
    static getBmi(weight, height) {
        return weight / (height * height);
    }
}

console.log(Util.VERSION);           // "1.0"
console.log(Util.getBmi(81, 1.76));  // 26.14...
```

- 위와 같은 방식으로 static을 명시적으로 선언할 수 있습니다.

## 📌 요약: JavaScript에서 static처럼 동작하는 패턴

| 표현 방식                        | 설명                                      |
|----------------------------------|-------------------------------------------|
| `Util.VERSION = "1.0"`           | 생성자 함수에 직접 속성 추가 → static 변수처럼 동작 |
| `Util.getBmi = function(...)`    | 함수에 직접 메서드 추가 → static 메서드처럼 동작 |
| `new Util()`                     | 인스턴스에는 static 속성이 없음           |
| `class Util { static ... }`      | ES6 이후 공식 static 문법 지원             |

---


