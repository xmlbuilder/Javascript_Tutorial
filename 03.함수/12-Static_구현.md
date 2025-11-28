# Static 구현

JavaScript에서 static 변수와 메서드, 그리고 **privileged method(특권 메서드)** 를 구현하는 방법을 보여주는 아주 좋은 예제입니다.  
아래에 그 개념들을 자세히 설명.

## 📦 JavaScript에서 static 개념이란?
JavaScript는 클래스 기반 언어가 아니기 때문에 전통적인 의미의 static 속성은 없습니다.  
하지만 **클로저(closure)** 와 **즉시 실행 함수(IIFE)** 를 활용하면 생성자 함수 외부에 공유되는 변수나 메서드를 만들 수 있습니다.  
이들이 사실상 static 변수/메서드처럼 동작합니다.

## 🔧 코드 구조 분석
### 🔹 STATIC 1: 기본 static 변수
```javascript
var Gadget = (function () {
  var counter = 0; // static 변수처럼 동작
  return function () {
    console.log(counter++); // 호출할 때마다 증가
  };
})();
```
- counter는 Gadget 생성자 외부에 정의되어 있지만, 클로저를 통해 내부에서 접근 가능
- Gadget()을 호출할 때마다 counter가 증가 → static 변수처럼 동작

### 🔹 STATIC 2: 생성자 + 특권 메서드
```javascript
var Gadget = (function () {
  var counter = 0;   // static 변수
  var lastId = 0;    // static 변수

  return function () {
    this.id = ++counter;
    lastId = this.id;

    // privileged method: 클로저를 통해 static 변수 접근
    this.getLastId = function () {
      return lastId;
    };
  };
})();
```

- counter, lastId는 외부에서 접근 불가 → 캡슐화된 static 변수
- getLastId()는 생성자 내부에서 정의된 privileged method로, 클로저를 통해 lastId에 접근 가능

### 🧪 실행 결과
```javascript
var iPhone = new Gadget(); // id: 1
console.log(iPhone.getLastId()); // 1

var iPod = new Gadget();   // id: 2
console.log(iPod.getLastId()); // 2

var iPad = new Gadget();   // id: 3
console.log(iPad.getLastId()); // 3
```

- 각 객체는 자신만의 getLastId() 메서드를 가지고 있지만, 내부적으로는 공유된 static 변수를 참조
- lastId는 마지막으로 생성된 객체의 id를 기억함

## 🧠 핵심 개념 요약

| 개념               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| static 변수         | 모든 인스턴스가 공유하며 생성자 외부에서 정의된 변수 (클로저로 은닉 가능) |
| static 메서드       | 클래스 또는 생성자 함수 자체에 직접 정의된 메서드                     |
| privileged method  | 생성자 내부에서 정의되어 클로저를 통해 static 변수에 접근 가능한 메서드 |
| 캡슐화              | 외부에서 직접 접근할 수 없도록 내부 데이터를 보호하는 설계 방식         |
| 클로저              | 함수가 외부 스코프의 변수에 접근할 수 있게 해주는 구조                 |



## ✅ 정리
JavaScript에서는 class 없이도 클로저와 IIFE를 활용해 static 변수와 메서드를 구현할 수 있습니다.  
이 방식은 객체지향 설계에서 중요한 공유 상태 관리와 정보 은닉을 동시에 만족시킬 수 있음.

---


## 📘 JavaScript & TypeScript에서 static과 캡슐화 구현하기
### 🔧 1. JavaScript 클래스 기반 static 구현
```javascript
class Gadget {
  static counter = 0; // static 변수

  constructor() {
    this.id = ++Gadget.counter;
  }

  getId() {
    return this.id;
  }

  static getCounter() {
    return Gadget.counter;
  }
}
```

#### ✅ 특징
- Gadget.counter는 클래스 자체에 속한 static 변수
- getCounter()는 클래스에 직접 접근하는 static 메서드
- getId()는 인스턴스 메서드
#### 🧪 사용 예시
```javascript
const g1 = new Gadget();
const g2 = new Gadget();

console.log(g1.getId());        // 1
console.log(g2.getId());        // 2
console.log(Gadget.getCounter()); // 2
```


### 🔧 2. TypeScript 클래스 기반 static 구현
```typescript
class Gadget {
  private static counter: number = 0; // static + 캡슐화

  private id: number;

  constructor() {
    this.id = ++Gadget.counter;
  }

  public getId(): number {
    return this.id;
  }

  public static getCounter(): number {
    return Gadget.counter;
  }
}
```

#### ✅ 특징
- private static counter: 외부에서 직접 접근 불가 → 캡슐화 강화
- 타입 명시 (number)로 안정성 확보
- public 키워드로 인터페이스 명확화
#### 🧪 사용 예시
```typescript
const g1 = new Gadget();
const g2 = new Gadget();

console.log(g1.getId());           // 1
console.log(g2.getId());           // 2
console.log(Gadget.getCounter());  // 2
```


## 🧠 핵심 비교 요약
| 항목               | JavaScript 클래스                  | TypeScript 클래스                     |
|--------------------|------------------------------------|----------------------------------------|
| static 변수 선언    | `static counter = 0`               | `private static counter: number = 0`   |
| 접근 제어          | 제한 없음                          | `private`, `public`으로 명확히 구분     |
| 타입 안정성        | 없음                               | `number`, `string` 등으로 명시적 선언   |
| 캡슐화 수준        | 기본적인 보호                      | 강력한 보호 (외부 접근 완전 차단 가능) |
| 메서드 정의 방식   | 클래스 내부                        | 클래스 내부 + 타입 명시                |



## 📦 확장 아이디어
- Gadget에 resetCounter() static 메서드 추가
- getId()를 readonly 속성으로 변경
- interface 또는 abstract class로 구조 일반화

---

