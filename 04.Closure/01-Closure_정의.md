# JavaScript의 클로저(Closure)에 대한 개념

## 📘 JavaScript 클로저(Closure) 완전 정리
### 1. 클로저란 무엇인가요?
클로저는 함수와 그 함수가 선언될 당시의 렉시컬 환경(Lexical Environment)의 조합입니다.

- 즉, 함수가 선언될 때의 **변수 범위(scope)** 를 기억하는 특별한 구조입니다.
- 함수가 반환되거나 다른 곳으로 전달되더라도, 자신이 선언된 위치의 변수들을 계속 참조할 수 있습니다.
- 이 변수들은 스냅샷이 아니라 살아있는 연결입니다. 값이 바뀌면 클로저에서도 바뀐 값을 볼 수 있습니다.

### 2. 클로저가 담고 있는 것
- 렉시컬 환경: 함수가 선언된 위치에서 접근 가능한 모든 변수, 상수, 함수 등
- 지속되는 상태: 외부 함수가 종료된 이후에도 내부 함수는 그 변수들을 계속 사용할 수 있음
- 접근 권한: 외부에서는 직접 접근할 수 없고, 클로저를 통해서만 접근 가능

### 3. 클로저의 장점
- ✅ 접근 권한 제어: 외부에서 직접 접근하지 못하게 하고, 필요한 기능만 노출
- ✅ 지역 변수 보호: 외부에서 변수 값을 임의로 바꾸는 것을 방지
- ✅ 데이터 보존 및 활용: 함수가 호출된 이후에도 상태를 유지하며 활용 가능

### 4. 클로저 예제
```javascript
function makeCounter(start = 0) {
  let count = start;

  return {
    inc() { count += 1; return count; },
    dec() { count -= 1; return count; },
    get() { return count; }
  };
}
```
```javascript
const c = makeCounter(10);
console.log(c.get()); // 10
console.log(c.inc()); // 11
console.log(c.dec()); // 10
```

- count는 외부에서 직접 접근할 수 없고, inc, dec, get 함수만을 통해 조작 가능
- 이 함수들은 count를 기억하고 있으므로 클로저입니다

## 5. 샘플 코드 분석
### 잘못된 클로저 활용
```javascript
function a() {
    var x = 10;
    return function() {
        setX = function() {
            return x;
        };
    };
}
var c = a();
c.x = 10;
console.log(c.x);
```

- a()는 내부에 x = 10을 선언하고, 함수 하나를 반환합니다
- 반환된 함수는 setX라는 전역 함수에 x를 반환하는 함수를 할당합니다
- c.x = 10은 c가 함수이므로 단순히 속성을 추가한 것일 뿐, x와는 무관합니다
### 올바른 클로저 활용 예시
```rust
function a() {
  let x = 10;

  return {
    getX() { return x; },
    setX(v) { x = v; }
  };
}
```
```rust
const c = a();
console.log(c.getX()); // 10
c.setX(42);
console.log(c.getX()); // 42
```
- x는 외부에서 직접 접근할 수 없고, getX, setX를 통해서만 접근 가능
- x는 a()의 렉시컬 환경에 존재하며, 클로저를 통해 유지됨

## 6. 자주 하는 실수와 주의점
- 🔸 var vs let in 반복문
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); // 3, 3, 3
}
```
```javascript
for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 0); // 0, 1, 2
}
```
- var는 함수 스코프, let은 블록 스코프 → 클로저에 영향을 줌
- 🔸 this와 클로저 혼동
  - 클로저는 변수 환경을 기억하지만, this는 실행 시점에 결정됨
  - 화살표 함수는 this를 렉시컬하게 캡처함
- 🔸 메모리 누수 주의
  - 클로저는 참조된 환경을 유지하므로, 오래 살아있는 클로저는 메모리 누수의 원인이 될 수 있음

## 7. 클로저를 활용한 패턴
- 모듈 패턴: 내부 상태를 숨기고 외부에 필요한 기능만 노출
- 팩토리 함수: 설정값을 기억하는 함수 생성
- 메모이제이션: 계산 결과를 기억해 성능 향상
- 커링/부분 적용: 일부 인자를 미리 고정한 함수 생성

## 🔑 기억할 것
- 함수는 선언된 위치의 변수들을 기억한다
- 클로저는 상태를 유지하고 보호하는 도구다
- 클로저를 잘 활용하면 더 안전하고 유연한 코드를 만들 수 있다

---

