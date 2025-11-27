# JavaScrpt 클로저 실행 절차
JavaScript 클로저의 실행 절차를 코드로 설명 및 클로저의 생성과 실행 흐름을 단계별로 문서화.

## 📘 JavaScript 클로저 실행 절차 정리
### 1. 클로저란?
클로저는 함수와 그 함수가 선언될 당시의 **렉시컬 환경(Lexical Environment)** 의 조합입니다.  
즉, 함수가 선언된 위치의 변수들을 기억하고, 그 이후에도 계속 접근할 수 있는 구조입니다.


### 2. 클로저 생성 예시
```javascript
function setName(name){
    return function(){
        return name;
    }
}
```
```javascript
var sayMyName = setName("Bear");
console.log(sayMyName()); // Bear
```

#### 🔍 클로저 실행 절차

| 단계              | 동작 설명                                      |
|-------------------|------------------------------------------------|
| setName("Bear")   | `name` 변수에 "Bear" 저장                      |
| 내부 함수 생성     | `function() { return name; }` → `name`을 기억하는 클로저 생성 |
| 함수 반환 및 저장 | 반환된 클로저를 `sayMyName` 변수에 저장         |
| sayMyName() 호출  | 클로저가 `name`을 기억하고 있으므로 "Bear" 반환 |

- ✅ 이 구조는 name이라는 지역 변수가 setName이 종료된 이후에도 살아남아 있다는 것을 보여줍니다.

### 3. 클로저로 상태 유지하기
```jvascript
function setCounter() {
    var count = 0;
    return function() {
        count++;
        return count;
    }
}
```
```javascript
var count = setCounter();
console.log(count()); // 1
console.log(count()); // 2
console.log(count()); // 3
```

#### 🔍 클로저 실행 절차: setCounter 예제

| 단계            | 동작 설명                                                  |
|-----------------|------------------------------------------------------------|
| setCounter()    | `count = 0` 초기화                                          |
| 내부 함수 생성   | `function() { count++; return count; }` → `count`를 기억하는 클로저 생성 |
| 함수 반환 및 저장 | 반환된 클로저를 `count` 변수에 저장                         |
| count() 호출    | 클로저가 `count`를 기억하고 있으므로 호출마다 `count++` 수행 |

- ✅ 이 구조는 클로저가 지역 변수 보호와 상태 보존을 동시에 수행하는 대표적인 예시입니다.

### 4. 클로저 실행 흐름 요약
```
[외부 함수 호출]
     ↓
[지역 변수 생성]
     ↓
[내부 함수 생성 → 클로저]
     ↓
[내부 함수 반환 → 외부 저장]
     ↓
[내부 함수 실행 → 지역 변수 접근 가능]
```

- 클로저는 외부 함수의 실행이 끝난 후에도 내부 변수에 접근할 수 있음
- 이는 JavaScript의 함수형 프로그래밍과 모듈화에 매우 유용한 구조입니다

## ✅ 클로저의 장점

| 기능               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| 접근 권한 제어       | 외부에서 직접 접근할 수 없고, 클로저를 통해서만 내부 변수에 접근 가능                     |
| 지역 변수 보호       | 함수 내부에서 선언된 변수는 외부에서 접근 불가 → 데이터 은닉 가능                         |
| 상태 보존 및 활용    | 함수가 종료된 이후에도 내부 변수 상태를 유지하며 활용 가능 → 상태 유지형 함수 구현 가능 |
| 모듈화 및 캡슐화     | 클로저를 통해 내부 로직을 숨기고 필요한 기능만 외부에 노출 → API 설계에 유리               |

---
# Closure Pattern

클로저를 활용하면 JavaScript에서 모듈화, 정보 은닉, 상태 유지형 API 설계가 매우 깔끔하게 가능합니다.  
아래에 대표적인 예시들을 정리.

## 📦 1. 모듈 패턴 (Module Pattern)
```javascript
const CounterModule = (function () {
  let count = 0; // private 변수

  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
})();
```
```javascript
console.log(CounterModule.getCount()); // 0
console.log(CounterModule.increment()); // 1
console.log(CounterModule.increment()); // 2
console.log(CounterModule.decrement()); // 1
```

### ✅ 특징
- count는 외부에서 직접 접근 불가 → 정보 은닉
- increment, decrement, getCount만 공개됨 → 접근 제어
- 클로저를 통해 count 상태가 유지됨 → 상태 보존

## 🔌 2. API 설계 예시: 사용자 세션 관리
```javascript
function createSession(userId) {
  let token = `token-${userId}-${Date.now()}`;

  return {
    getToken() {
      return token;
    },
    refreshToken() {
      token = `token-${userId}-${Date.now()}`;
      return token;
    }
  };
}
```
```javascript
const session = createSession("user123");
console.log(session.getToken());       // token-user123-...
console.log(session.refreshToken());   // 새로운 토큰
console.log(session.getToken());       // 갱신된 토큰
```

### ✅ 특징
- token은 외부에서 직접 수정 불가 → 보안 강화
- getToken, refreshToken만 공개 → API 설계에 적합
- 클로저로 userId와 token 상태를 유지 → 세션 관리에 유용

## 🧪 3. 클로저 기반 설정 저장기
```javascript
function createConfig(defaults) {
  let config = { ...defaults };

  return {
    get(key) {
      return config[key];
    },
    set(key, value) {
      config[key] = value;
    },
    reset() {
      config = { ...defaults };
    }
  };
}
```
```javascript
const appConfig = createConfig({ theme: "dark", lang: "ko" });
console.log(appConfig.get("theme")); // dark
appConfig.set("theme", "light");
console.log(appConfig.get("theme")); // light
appConfig.reset();
console.log(appConfig.get("theme")); // dark
```

### ✅ 특징
- config는 외부에서 직접 접근 불가 → 설정 보호
- get, set, reset으로만 조작 가능 → API 형태로 설계
- 클로저로 초기값과 현재 상태를 모두 기억 → 유연한 설정 관리

## ✨ 요약

| 패턴/기능        | 활용 목적                          | 클로저의 역할                          |
|------------------|-----------------------------------|----------------------------------------|
| 모듈 패턴        | 내부 상태 보호, 필요한 기능만 노출   | private 변수 유지, 접근 제어             |
| API 설계         | 상태 기반 기능 제공                 | 세션, 설정, 사용자 정보 등 보존          |
| 설정/캐시 관리    | 초기값 기반 동적 상태 관리           | 초기값 기억 + 현재값 유지                |
| 데이터 은닉       | 외부 접근 차단, 보안 강화            | 지역 변수 보호, 클로저를 통한 제한적 접근 |





----



# 📘 클로저의 메모리 구조와 성능 최적화
## 1. 클로저의 메모리 구조
- 렉시컬 환경(Lexical Environment) 캡처
    - 클로저는 함수가 선언될 당시의 스코프 체인을 기억합니다.
    - 이때 필요한 변수뿐 아니라 해당 스코프 전체 환경을 캡처합니다.
    - 따라서 사용하지 않는 변수도 메모리에 남아 있을 수 있습니다.
- 변수 생명 주기 연장
    - 일반적으로 함수가 종료되면 지역 변수는 해제됩니다.
    - 하지만 클로저가 참조하고 있는 변수는 **GC(Garbage Collector)** 가 해제하지 못합니다.
    - 클로저가 살아 있는 동안 해당 변수와 환경은 메모리에 유지됩니다.
- 참조 구조
    - 클로저 함수 → [[Environment]] → 외부 함수의 변수들
    - 내부 함수는 숨겨진 [[Environment]] 링크를 통해 외부 변수에 접근합니다.
    - 이 링크가 존재하는 한, 외부 함수의 컨텍스트는 해제되지 않습니다.

## 2. 성능 최적화 관점
### ⚠️ 잠재적 문제
- 메모리 누수 위험
    - 클로저가 DOM 요소나 대규모 객체를 참조하면, 해당 객체가 해제되지 않아 메모리 누수가 발생할 수 있습니다.
- 불필요한 환경 유지
    - 클로저가 실제로 사용하지 않는 변수까지 캡처하여 메모리 낭비 가능성이 있습니다.
- 장시간 실행되는 앱
    - SPA(싱글 페이지 앱)처럼 클로저가 많이 쌓이면 성능 저하로 이어질 수 있습니다.

### ✅ 최적화 전략
| 전략                | 설명                                                                 |
|---------------------|----------------------------------------------------------------------|
| 불필요한 참조 제거    | 더 이상 필요 없는 클로저나 변수는 `null`로 설정하여 GC가 해제할 수 있도록 함 |
| 캡처 범위 최소화      | 실제로 필요한 변수만 캡처하도록 설계, 불필요한 외부 참조 피하기                 |
| 이벤트 핸들러 관리    | `removeEventListener`를 사용해 클로저가 연결된 핸들러를 적절히 해제              |
| 모듈화 및 격리       | 클로저를 모듈 내부에 한정시켜 외부 노출 최소화                                |
| 디버깅 도구 활용      | Chrome DevTools Memory 탭 등으로 클로저와 참조된 객체를 추적하여 메모리 누수 방지 |



## 3. Rust와의 비교 (안전성 관점)
- JavaScript: GC 기반, 클로저가 환경 전체를 유지 → 메모리 누수 위험
- Rust: 소유권과 라이프타임 기반, 필요한 변수만 캡처 → 스코프 벗어나면 자동 해제
- 따라서 Rust 클로저는 메모리 관리가 더 안전하고 예측 가능

## ✅ 결론
- 클로저는 상태 유지와 정보 은닉에 매우 유용하지만,
- 메모리 구조상 환경 전체를 캡처하기 때문에 성능 최적화에 주의가 필요합니다.
- 올바른 관리(참조 해제, 범위 최소화, 모듈화)를 통해 클로저를 안전하게 활용할 수 있습니다.

---


