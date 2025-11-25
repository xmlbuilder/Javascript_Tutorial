# map

## 📝 map() 메서드란?
- 배열의 각 요소를 변환해서 새로운 배열을 반환하는 메서드.
- 원본 배열은 변경되지 않음.
- 콜백 함수가 배열의 각 요소에 대해 실행되며, 그 결과값으로 새로운 배열을 구성.

## 🔧 문법
```rust
array.map(function(element, index, array) {
  // return 변환된 값
});
```

- element: 현재 처리 중인 요소 값
- index: 현재 요소의 인덱스
- array: 원본 배열 전체

## 📊 특징
- 반환값: 새로운 배열
- 원본 배열: 그대로 유지
- 길이: 원본 배열과 동일한 길이의 배열 반환
- 주로 데이터 변환, 가공에 사용

## 💡 예제
### 1. 제곱 계산
```javascript
var data = [1, 2, 3];
var squares = data.map(x => x * x);

console.log(squares); // [1, 4, 9]
console.log(data);    // [1, 2, 3] → 원본은 그대로
```

### 2. 문자열 변환
```javascript
var names = ["kim", "lee", "park"];
var upper = names.map(name => name.toUpperCase());

console.log(upper); // ["KIM", "LEE", "PARK"]
```

### 3. 객체 배열 변환
```javascript
var users = [
  { id: 1, name: "Kim" },
  { id: 2, name: "Lee" }
];

var ids = users.map(user => user.id);

console.log(ids); // [1, 2]
```


## 📌 핵심 정리
| 특징              | 설명                                   |
|-------------------|--------------------------------------|
| 원본 배열 변경 여부 | ❌ 변경되지 않음                        |
| 반환값             | ✅ 새로운 배열                         |
| 길이               | 원본 배열과 동일                        |
| 주요 용도           | 데이터 변환, 가공, 새로운 배열 생성       |



## 👉 요약:
- map()은 배열을 **가공 공장** 에 넣어 새로운 배열을 만들어내는 도구입니다.
- 원본은 그대로 두고, 변환된 결과만 새 배열로 반환하기 때문에 데이터 처리에 매우 자주 쓰임.

---

