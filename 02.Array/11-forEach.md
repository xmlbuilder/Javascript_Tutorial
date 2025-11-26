# forEach

JavaScript의 forEach() 메서드에 대한 정리입니다.  
예제와 함께 기능 설명, 문법, 활용 팁까지 깔끔하게 정리.

## 📘 JavaScript forEach() 메서드 정리
### ✅ 개요
forEach()는 배열의 각 요소에 대해 지정한 콜백 함수를 한 번씩 실행하는 메서드입니다.  
반환값은 없으며, 주로 **출력, 누적, DOM 처리 등 부수 효과(side effect)** 를 위해 사용됩니다.

### 🧠 문법
```javascript
array.forEach(function(element, index, array) {
    // 실행할 코드
});
```

- 또는 ES6 화살표 함수로:

```javascript
array.forEach((element) => {
    // 실행할 코드
});
```
- element: 현재 요소
- index (선택): 현재 인덱스
- array (선택): 원본 배열

### 🧪 예제
#### 🔹 기본 사용
```javascript
var data = ['a', 'b', 'c', 'd'];

data.forEach((val) => {
    console.log("Output Data", val);
});
// 출력:
// Output Data a
// Output Data b
// Output Data c
// Output Data d
```

#### 🔹 인덱스 포함 출력
```javascript
data.forEach((val, idx) => {
    console.log(`Index ${idx}: ${val}`);
});
// 출력:
// Index 0: a
// Index 1: b
// Index 2: c
// Index 3: d
```


## 📊 시각적 요약
| 배열           | 콜백 내용                  | 결과 출력                      |
|----------------|----------------------------|--------------------------------|
| ['a','b','c']  | console.log(val)           | a, b, c                        |
| ['x','y']      | console.log(idx + val)     | 0x, 1y                         |
| [1,2,3]        | sum += val                 | 누적합 계산 가능               |


## 💡 활용 팁
- forEach()는 반환값이 없으므로 데이터 변환에는 부적합 → map() 사용
- break, return으로 반복 중단 불가 → 중단이 필요하면 for 또는 some() 사용
- DOM 조작, 로그 출력, 누적 계산 등 부수 효과 작업에 적합

## ❗ 주의사항
- forEach()는 비동기 함수와 함께 사용할 때 주의 필요 (예: await는 작동하지 않음)
- undefined 요소도 순회 대상이 됨 (희소 배열은 건너뛰기도 함)

---

