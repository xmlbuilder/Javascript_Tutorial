# splice / slice
splice와 slice는 이름이 비슷해서 헷갈리기 쉬운데, 동작 방식은 완전히 다릅니다.  
예제와 함께 차이를 정리.

## ✂️ splice()
- 원본 배열을 직접 변경합니다.
- 특정 위치에서 요소를 추가/삭제할 때 사용.
- 반환값은 삭제된 요소들의 배열.
- 문법
  - start: 시작 인덱스
  - deleteCount: 제거할 요소 개수
  - item...: 그 자리에 새로 넣을 요소들 (옵션)
```javascript
array.splice(start, deleteCount, item1, item2, ...)
```
- 예시
```javascript
var data = ["a", "b", "c", "d", "e", "f", "g", "h"];

console.log(data.splice(2, 3)); 
// ['c', 'd', 'e'] → 삭제된 요소 반환
console.log(data); 
// ['a', 'b', 'f', 'g', 'h'] → 원본 배열 변경됨
```
```javascript
var data2 = ["a", "b", "c", "d", "e", "f", "g", "h"];
console.log(data2.splice(2, 3, '1', '2', '3')); 
// ['c', 'd', 'e'] → 삭제된 요소 반환
console.log(data2); 
// ['a', 'b', '1', '2', '3', 'f', 'g', 'h'] → 원본 배열 변경 + 새 요소 삽입
```

## 🍰 slice()
- 원본 배열은 변경하지 않음.
- 배열의 일부를 잘라서 새로운 배열을 반환.
- 주로 배열 복사나 부분 추출에 사용.
- 문법
  - start: 시작 인덱스 (포함)
  - end: 끝 인덱스 (미포함)
```javascript
array.slice(start, end)
```   
- 예시
```javascript
var data = ["a", "b", "c", "d", "e"];

console.log(data.slice(1));     
// ['b', 'c', 'd', 'e'] → 1번부터 끝까지
console.log(data);              
// ['a', 'b', 'c', 'd', 'e'] → 원본 배열 그대로

console.log(data.slice(1, 3));  
// ['b', 'c'] → 1번부터 3번 직전까지
```


## 📌 핵심 차이 정리

| 메서드   | 원본 배열 변경 여부 | 반환값             | 주요 용도           |
|----------|------------------|------------------|------------------|
| splice   | ✅ 변경됨          | 삭제된 요소 배열     | 요소 삭제/추가       |
| slice    | ❌ 변경 안 됨      | 잘라낸 부분 배열     | 배열 복사/부분 추출   |


## 👉 기억하기 쉽게:
- splice = 수술칼 → 원본 배열을 직접 잘라내고 꿰맨다.
- slice = 케이크 조각 → 원본은 그대로 두고 조각만 가져온다.
