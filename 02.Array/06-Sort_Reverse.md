# Sort / Reverse

## 🔢 sort()
- 배열을 정렬하는 메서드.
- 원본 배열을 직접 변경합니다.
- 기본 동작은 **문자열 기준(유니코드 순서)** 으로 정렬하기 때문에 숫자 배열에서는 예상과 다른 결과가 나올 수 있음.
- 기본 사용
```javascript
var data = [7, 38, 21];
console.log(data.sort()); 
// [21, 38, 7] → 문자열 비교로 정렬됨
```

- "21" < "38" < "7" 순서로 판단해서 이런 결과가 나옴.
## 숫자 정렬 (비교 함수 사용)
```javascript
var data = [7, 38, 21];
console.log(data.sort((m, n) => m - n)); 
// [7, 21, 38] → 오름차순 정렬
```    
- (m - n) → 양수면 자리 바꿈, 음수면 그대로 둠.
- 내림차순은 (n - m) 사용.

## 🔄 reverse()
- 배열의 요소 순서를 뒤집음.
- 역시 원본 배열을 직접 변경합니다.
- 예시
```javascript
var data = [21, 38, 7];
console.log(data.reverse()); 
// [7, 38, 21]
```

- 단순히 배열을 역순으로 뒤집음.

## 📌 핵심 차이 정리
| 메서드    | 원본 배열 변경 여부 | 반환값        | 주요 용도                  |
|-----------|------------------|-------------|-------------------------|
| sort()    | ✅ 변경됨          | 정렬된 배열    | 배열 정렬 (문자열/숫자)     |
| reverse() | ✅ 변경됨          | 뒤집힌 배열    | 배열 순서 뒤집기            |



## 👉 기억하기 쉽게:
- sort = 정렬기 → 기준에 따라 배열을 재배치.
- reverse = 뒤집기 버튼 → 배열을 거꾸로 뒤집음.

---

# slice
.slice().sort()와 .slice().reverse()는 원본 배열을 건드리지 않고 정렬/뒤집기를 할 수 있는 패턴입니다.

## 📝 .slice()
- slice()는 배열의 일부를 복사하거나, 인자를 생략하면 전체 배열을 복사합니다.
- 즉, arr.slice() → 원본 배열의 **얕은 복사본(shallow copy)** 을 반환.

## 🔢 .slice().sort()
```javascript
var data = [7, 38, 21];
var sorted = data.slice().sort((a, b) => a - b);

console.log(sorted); // [7, 21, 38]
console.log(data);   // [7, 38, 21] → 원본은 그대로
```      

- slice()로 복사본을 만든 뒤 sort()를 적용.
- 원본 배열은 변경되지 않고, 정렬된 새로운 배열을 얻을 수 있음.
- 불변성 유지가 필요한 상황(예: React 상태 관리)에서 자주 쓰임.

## 🔄 .slice().reverse()
```javascript
var data = [7, 38, 21];
var reversed = data.slice().reverse();

console.log(reversed); // [21, 38, 7]
console.log(data);     // [7, 38, 21] → 원본은 그대로
```

- slice()로 복사본을 만든 뒤 reverse()를 적용.
- 원본 배열은 변경되지 않고, 뒤집힌 새로운 배열을 얻을 수 있음.

## 📌 핵심 차이 정리
| 패턴              | 원본 배열 변경 여부 | 반환값             | 주요 용도                  |
|-------------------|------------------|------------------|-------------------------|
| .sort()           | ✅ 변경됨          | 정렬된 배열         | 원본 배열을 직접 정렬        |
| .reverse()        | ✅ 변경됨          | 뒤집힌 배열         | 원본 배열을 직접 뒤집기      |
| .slice().sort()   | ❌ 변경 안 됨      | 정렬된 복사본 배열   | 원본 유지 + 정렬 결과 얻기   |
| .slice().reverse()| ❌ 변경 안 됨      | 뒤집힌 복사본 배열   | 원본 유지 + 역순 결과 얻기   |



## 👉 요약:
- 원본 배열을 그대로 두고 싶을 때는 .slice()를 먼저 붙인다.
- 그 뒤 .sort()나 .reverse()를 적용하면 불변성을 유지하면서 새로운 배열을 얻을 수 있다.


----


## 🔽 내림차순 정렬
### 1. sort() + 비교 함수
```javascript
var data = [7, 38, 21];
var desc = data.slice().sort((a, b) => b - a);

console.log(desc);  // [38, 21, 7]
console.log(data);  // [7, 38, 21] → 원본은 그대로
```

### 2. 오름차순 정렬 후 reverse()
```javascript
var data = [7, 38, 21];
var desc = data.slice().sort((a, b) => a - b).reverse();

console.log(desc);  // [38, 21, 7]
```


## 📋 배열 복사/변환 방법
### 1. slice()
```javascript
var arr = [1, 2, 3];
var copy = arr.slice();

console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false (다른 배열)
```

## 2. Array.from()
```javascript
var arr = [1, 2, 3];
var copy = Array.from(arr);

console.log(copy); // [1, 2, 3]
```
- Array.from()은 유사 배열 객체나 이터러블도 배열로 변환 가능.
```javascript
var str = "hello";
console.log(Array.from(str)); // ['h','e','l','l','o']
```

## 3. Array.of()
```javascript
var arr = Array.of(7, 38, 21);
console.log(arr); // [7, 38, 21]
```

- 전달된 인자를 그대로 요소로 갖는 배열 생성.
- new Array(3)처럼 길이로 해석되지 않고 항상 요소로 취급.
## 4. Spread 연산자 ...
```javascript
var arr = [1, 2, 3];
var copy = [...arr];

console.log(copy); // [1, 2, 3]
```

- 가장 간단하고 직관적인 복사 방법.
- 다른 배열과 합치거나 새로운 요소 추가도 가능:
```javascript
var merged = [...arr, 4, 5];
console.log(merged); // [1, 2, 3, 4, 5]
```

## 5. map() 변환
```javascript
var arr = [1, 2, 3];
var doubled = arr.map(x => x * 2);

console.log(doubled); // [2, 4, 6]
```

- 복사와 동시에 변환 가능.

## 📌 핵심 정리
| 방법            | 특징                                   |
|-----------------|--------------------------------------|
| slice()         | 배열 복사, 부분 추출 가능               |
| Array.from()    | 유사 배열/이터러블을 배열로 변환 가능    |
| Array.of()      | 인자를 그대로 요소로 갖는 배열 생성      |
| Spread (...)    | 직관적 복사, 합치기/추가에 유용          |
| map()           | 복사 + 변환 동시에 수행                 |

## 👉 요약:
- 내림차순 정렬은 (b - a) 비교 함수나 reverse() 조합으로 구현.
- 배열 복사/변환은 slice, Array.from, Array.of, spread, map 등 다양한 방식이 있으며 상황에 맞게 선택하면 됨.
---

