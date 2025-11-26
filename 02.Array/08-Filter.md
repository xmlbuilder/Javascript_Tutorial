## 📌 filter()란?
filter()는 배열에서 조건을 만족하는 요소만 골라 새로운 배열을 만드는 메서드입니다.  
원본 배열은 변경되지 않고, 조건을 만족하는 요소들만 추출됩니다.  

## 🧠 기본 문법
```javascript
let result = array.filter(function(element, index, array) {
    return 조건식;
});
```

### 또는 ES6 화살표 함수로:
```javascript
let result = array.filter((element) => 조건식);
```

- element: 배열의 현재 요소
- index (선택): 현재 인덱스
- array (선택): 원본 배열

## ✅ 예제 모음
### 1. 길이가 6보다 큰 단어만 추출
```javascript
var words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];
var result = words.filter(word => word.length > 6);
console.log(result); // ["exuberant", "destruction", "present"]
```

### 2. 길이가 6보다 작은 단어만 추출
```javascript
var result = words.filter(function(word){
    return word.length < 6;
});
console.log(result); // ["spray", "limit", "elite"]
```

### 3. 숫자 배열에서 1보다 큰 값만 추출
```javascript
var data = [1, 2, 3];
var data2 = data.filter(function(data){
    return data > 1;
});
console.log(data2); // [2, 3]
```

### 4. 화살표 함수로 간단하게
```javascript
var data = [1, 2, 3, 4, 5];
var data2 = data.filter(value => value > 1);
console.log(data2); // [2, 3, 4, 5]
```


## 🧪 활용 팁
- filter()는 조건에 맞는 요소만 남기므로 검색, 필터링, 정제 작업에 매우 유용합니다.
- map()과 함께 쓰면 조건에 맞는 요소를 변형해서 추출할 수 있습니다.
- filter() 후 .length를 쓰면 조건을 만족하는 개수도 쉽게 구할 수 있습니다.

## 📌 시각적 요약

| 원본 배열             | 조건식                  | 결과 배열                          |
|----------------------|-------------------------|------------------------------------|
| [1, 2, 3]            | value > 1               | [2, 3]                             |
| ['spray', ...]       | word.length > 6         | ['exuberant', 'destruction']       |
| [1, 2, 3, 4, 5]      | value % 2 === 0         | [2, 4]                             |

---


