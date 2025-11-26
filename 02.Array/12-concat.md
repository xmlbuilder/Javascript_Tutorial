# concat
아래는 concat() 메서드에 대한 정리입니다.  
push()와의 차이점까지 포함해서 기능 설명, 문법, 활용 팁을 깔끔하게 정리.

## 📘 JavaScript concat() 메서드 정리
### ✅ 개요
concat()은 하나 이상의 배열을 기존 배열에 병합하여 새로운 배열을 반환하는 메서드입니다.  
원본 배열은 변경되지 않으며, **immutable(불변성)** 을 유지합니다.

### 🧠 문법
```javascript
let newArray = array1.concat(array2, array3, ...);
```

- array1: 기준 배열
- array2, array3, ...: 병합할 배열들
- 반환값: 병합된 새 배열

### 🔹 push()와의 차이점

| 메서드     | 원본 배열 변경 여부 | 반환값            | 특징/용도                          |
|------------|---------------------|-------------------|------------------------------------|
| concat()   | ❌ 변경되지 않음     | 새 배열            | 불변성 유지, 여러 배열 병합 가능   |
| push()     | ✅ 변경됨            | 새 배열 길이       | 원본 배열에 요소 추가              |



### 🧪 예제
```javascript
var data1 = ['Sample1', 'Sample2', 'Sample3'];
var data2 = ['Sample4', 'Sample5', 'Sample6'];
var data3 = ['Sample7', 'Sample8', 'Sample9'];

var data4 = data1.concat(data2);
console.log(data4); 
// 출력: ['Sample1', 'Sample2', 'Sample3', 'Sample4', 'Sample5', 'Sample6']

var data5 = data1.concat(data2, data3);
console.log(data5);
/*
출력:
[
  'Sample1', 'Sample2', 'Sample3',
  'Sample4', 'Sample5', 'Sample6',
  'Sample7', 'Sample8', 'Sample9'
]
*/
```


## 📊 시각적 요약
| 원본 배열         | 병합 대상           | 결과 배열                                |
|------------------|---------------------|------------------------------------------|
| ['a','b']        | ['c','d']           | ['a','b','c','d']                         |
| ['1','2']        | ['3'], ['4','5']    | ['1','2','3','4','5']                     |
| ['x']            | []                  | ['x']                                     |


## 💡 활용 팁
- concat()은 여러 배열을 한 번에 병합할 수 있어 편리합니다.
- 원본 배열을 유지해야 하는 경우 concat()이 안전한 선택입니다.
- 객체나 숫자, 문자열도 배열처럼 병합 가능:
```javscript
['a'].concat('b'); // ['a', 'b']
[1, 2].concat([3], 4); // [1, 2, 3, 4]
```


## ❗ 주의사항
- concat()은 깊은 복사(deep copy)를 하지 않음 → 중첩 배열은 참조 유지
- push()는 원본 배열을 직접 변경하므로 상태 관리가 필요한 경우 주의

---



