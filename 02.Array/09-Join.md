# Joint

JavaScript의 join() 메서드를 정리한 문서입니다.  
join()은 배열을 문자열로 변환할 때 매우 유용한 메서드이며, 다양한 방식으로 활용할 수 있습니다.

## 📘 JavaScript join() 메서드 정리
### ✅ 개요
join()은 JavaScript 배열의 모든 요소를 하나의 문자열로 결합하는 메서드입니다.
각 요소 사이에 지정한 **구분자(separator)** 를 넣어 연결하며, 기본 구분자는 쉼표(,)입니다.

### 🧠 문법
```javascript
array.join(separator)
```
- separator: 각 요소 사이에 들어갈 문자열 (생략 시 기본값은 ',')
- 반환값: 연결된 문자열

### 🧪 예제 모음
#### 🔹 기본 사용
```javascript
var elements = ['Fire', 'Wind', 'Rain'];

console.log(elements.join());       // "Fire,Wind,Rain"
console.log(elements.join(''));     // "FireWindRain"
console.log(elements.join('-'));    // "Fire-Wind-Rain"
```

#### 🔹 한글 배열 예제
```javascript
var data = ['팬더','토끼','코알라'];

console.log(data.join(' '));        // "팬더 토끼 코알라"
console.log(data.toString());       // "팬더,토끼,코알라"
console.log(data);                  // [ '팬더', '토끼', '코알라' ]
```
- join(' '): 공백으로 연결
- toString(): 기본 쉼표로 연결
- console.log(data): 배열 그대로 출력

#### 🔹 빈 배열과 join() 활용
```javascript
var intent = 2;
var data = new Array(intent);       // [ <2 empty items> ]
console.log(data);                  // [ <2 empty items> ]

var str = data.join("---");
console.log(str);                   // "---"
```

- new Array(2)는 길이 2의 빈 배열을 생성
- join("---")은 요소가 없더라도 구분자만 1번 출력

#### 🔹 요소를 채운 후 join()
```javascript
data[0] = ' ';
data[1] = ' ';
str = data.join("---");
console.log(str);                   // " --- "
```

- 요소가 채워지면 구분자 사이에 값이 들어감
- " --- "는 ' ' + "---" + ' '의 결과

## 📊 시각적 요약
| 배열                        | 구분자       | 결과 문자열             |
|----------------------------|--------------|--------------------------|
| ['Fire', 'Wind', 'Rain']   | ',' (기본)   | "Fire,Wind,Rain"         |
| ['Fire', 'Wind', 'Rain']   | ''           | "FireWindRain"           |
| ['Fire', 'Wind', 'Rain']   | '-'          | "Fire-Wind-Rain"         |
| ['팬더','토끼','코알라']   | ' '          | "팬더 토끼 코알라"       |
| new Array(2)               | '---'        | "---"                    |
| [' ', ' ']                 | '---'        | " --- "                  |



## 💡 활용 팁
- join()은 CSV, 로그, 텍스트 출력 등 문자열 조합에 매우 유용합니다.
- 빈 배열에서도 구분자가 출력되므로 반복 문자열 생성에도 활용됩니다:
  - Array(4).join('*') // "***"
- join()은 toString()과 유사하지만, 구분자를 직접 지정할 수 있다는 점에서 더 유연합니다.

---

