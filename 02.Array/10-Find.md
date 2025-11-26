# find

아래는 find()와 findIndex() 메서드에 대한 정리입니다.  
예제와 함께 기능 설명, 문법, 활용 팁까지 깔끔하게 정리.

## 📘 JavaScript find() & findIndex() 메서드 정리
### ✅ 개요

| 메서드       | 설명                                                   |
|--------------|--------------------------------------------------------|
| find()       | 조건을 만족하는 첫 번째 요소를 반환                    |
| findIndex()  | 조건을 만족하는 첫 번째 요소의 인덱스를 반환           |

- 둘 다 조건을 만족하는 요소가 없으면 undefined 또는 -1을 반환합니다.

### 🧠 문법
```javascript
array.find(function(element, index, array) {
    return 조건식;
});
```
```javascript
array.findIndex(function(element, index, array) {
    return 조건식;
});
```

- ES6 람다 함수로도 사용 가능:
```javascript
array.find((element) => 조건식);
array.findIndex((element) => 조건식);
```


### 🧪 예제 모음
#### 🔹 한글 배열에서 찾기
```javascript
var animal = [
    '프렌치불독',
    '요크셔테리어',
    '닥스훈트',
    '포메마리언',
    '코키'
];

console.log(animal.find(function(value){
    return value === '포메마리언';
})); 
// 출력: "포메마리언"

console.log(animal.findIndex(function(value){
    return value === '포메마리언';
})); 
// 출력: 3
```

#### 🔹 영어 배열에서 찾기
```javascript
var data = ["Sample1","Sample2","Sample3","Sample4","Sample5","Sample6"];

console.log(data.find((val) => val === "Sample3")); 
// 출력: "Sample3"

console.log(data.findIndex((val) => val === "Sample3")); 
// 출력: 2
```


## 📊 시각적 요약
| 배열                             | 조건식                  | find 결과     | findIndex 결과 |
|----------------------------------|--------------------------|----------------|-----------------|
| ['A', 'B', 'C']                  | val === 'B'              | 'B'            | 1               |
| ['사과', '배', '귤']             | val === '귤'             | '귤'           | 2               |
| ['Sample1', ..., 'Sample6']      | val === 'Sample3'        | 'Sample3'      | 2               |
| ['프렌치불독', ..., '코키']      | val === '포메마리언'     | '포메마리언'   | 3               |

## 💡 활용 팁
- find()는 조건에 맞는 값을 직접 가져올 때 사용
- findIndex()는 위치를 알고 싶을 때 사용
- filter()는 조건을 만족하는 모든 요소를 가져오고, find()는 첫 번째 요소만 가져옵니다
- find()는 객체 배열에서도 유용하게 사용됩니다:
```javascript
let users = [
  { id: 1, name: 'JungHwan' },
  { id: 2, name: 'Minji' }
];

let user = users.find(u => u.id === 2);
console.log(user.name); // "Minji"
```
---




