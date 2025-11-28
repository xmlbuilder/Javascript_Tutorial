# OOP
JavaScript 코드 예제를 바탕으로 객체지향 프로그래밍(OOP) 개념 중심으로 문서화한 설명.  
Person과 Phone 객체를 통해 생성자 함수, 프로토타입, 메서드 정의 방식의 차이와 캡슐화 개념을 함께 다룹니다.

## 📘 JavaScript 객체지향 프로그래밍 예제 문서

### 🧠 핵심 개념 요약

| 개념             | 설명                                           |
|------------------|------------------------------------------------|
| 생성자 함수      | 객체를 생성하기 위한 함수. `new` 키워드와 함께 사용 |
| new              | 생성자 함수를 호출하여 새로운 객체 인스턴스를 생성 |
| 프로토타입       | 생성된 객체들이 공유하는 메서드를 정의하는 공간     |
| 인스턴스         | 생성자 함수를 통해 만들어진 객체 개별 실체           |
| 캡슐화           | 내부 로직이나 데이터를 외부에서 직접 접근하지 못하게 보호 |


## 👤 Person 객체: 프로토타입 기반 설계
```javascript
var Person = function(name, birth){
    this.name = name;
    this.birth = birth;
};

Person.prototype.getName = function(){
    return this.name;
};

Person.prototype.getBirth = function(){
    return this.birth;
};

Person.prototype.getAge = function(){
    var today = new Date();
    var birthDate = new Date(this.birth);
    var age = today.getFullYear() - birthDate.getFullYear();
    var m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())){
        age--;
    }
    return age;
};

Person.prototype.toString = function(){
    return "[Person " + this.getName() + "]";
};
```


## ✅ 특징
- getName, getBirth, getAge, toString은 프로토타입에 정의되어 모든 인스턴스가 공유
- 메모리 효율적이며, 객체지향적인 설계에 적합
- getAge()는 지역 변수를 활용하여 내부 로직을 외부에 노출하지 않음 → 캡슐화 실현

## 🧪 사용 예시
```javascript
var person = new Person("홍길동", "1990-01-01");

console.log(person.getName());     // 홍길동
console.log(person.getBirth());    // 1990-01-01
console.log(person.getAge());      // (현재 기준 나이)
console.log(person.toString());    // [Person 홍길동]
console.log(person);               // Person {name: "홍길동", birth: "1990-01-01"}
```


## 📱 Phone 객체: 인스턴스 메서드 기반 설계
```javascript
var Phone = function(name){
    this.name = name;
    this.setName = function(init){
        this.name = init;
    };
    this.getName = function(){
        return this.name;
    };
    this.toString = function(){
        return "[Phone " + this.getName() + "]";
    };
};
```

### ✅ 특징
- 모든 메서드가 생성자 내부에 정의되어 각 인스턴스마다 별도로 생성됨
- 메모리 사용량 증가 가능성 있음
- name 속성은 외부에서 직접 접근 가능 → 캡슐화가 약함

### 🧪 사용 예시
```javascript
var phone1 = new Phone("iphone");
var phone2 = new Phone("galaxy");
var phone3 = new Phone("ipod");

console.log(phone1.getName()); // iphone
console.log(phone2.getName()); // galaxy
console.log(phone3.getName()); // ipod

phone1.setName("nexus");
phone2.setName("google");
phone3.setName("htc");

console.log(phone1.getName()); // nexus
console.log(phone2.getName()); // google
console.log(phone3.getName()); // htc
```

## 🔍 비교 요약

| 항목              | Person 객체                      | Phone 객체                        | 비고                         |
|-------------------|----------------------------------|-----------------------------------|------------------------------|
| 메서드 정의 방식  | 프로토타입 기반                  | 생성자 내부 정의                  | 메모리 효율성 차이 있음      |
| 메서드 공유 여부  | 모든 인스턴스가 메서드 공유      | 인스턴스마다 메서드 개별 생성     |                              |
| 캡슐화 수준       | 높음 (`getAge` 내부 로직 은닉)   | 낮음 (`name` 직접 접근 가능)      |                              |
| 대표 메서드       | `getAge()`                       | `name` 속성 직접 접근 또는 `getName()` | 기능 접근 방식 차이          |
| 객체지향 설계     | 적합                              | 단순한 구조에 적합                | 확장성 고려 시 Person이 유리 |


## 📦 확장 아이디어
- Person과 Phone을 클래스 문법으로 리팩토링
- Person에 updateName() 같은 setter 메서드 추가
- Phone의 메서드를 프로토타입으로 분리하여 메모리 최적화

---

# class / typescript

JavaScript의 클래스 기반 객체 설계와 이를 TypeScript로 변환한 버전을 나란히 비교하며 설명한 문서입니다.  
Person과 Phone 객체를 예제로 사용해 객체지향 프로그래밍(OOP), 캡슐화, 타입 안정성까지 모두 다룹니다.

## 📘 클래스 기반 & TypeScript 객체 설계 예제
### 👤 Person 클래스 (JavaScript)
```javascript
class Person {
  constructor(name, birth) {
    this.name = name;
    this.birth = birth;
  }

  getName() {
    return this.name;
  }

  getBirth() {
    return this.birth;
  }

  getAge() {
    const today = new Date();
    const birthDate = new Date(this.birth);
    let age = today.getFullYear() - birthDate.getFullYear();
    const m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
      age--;
    }
    return age;
  }

  toString() {
    return `[Person ${this.getName()}]`;
  }
}
```

### ✅ 사용 예시
```javascript
const person = new Person("홍길동", "1990-01-01");
console.log(person.getName());     // 홍길동
console.log(person.getBirth());    // 1990-01-01
console.log(person.getAge());      // 현재 기준 나이
console.log(person.toString());    // [Person 홍길동]
```


### 👤 Person 클래스 (TypeScript)
```typescript
class Person {
  private name: string;
  private birth: string;

  constructor(name: string, birth: string) {
    this.name = name;
    this.birth = birth;
  }

  public getName(): string {
    return this.name;
  }

  public getBirth(): string {
    return this.birth;
  }

  public getAge(): number {
    const today = new Date();
    const birthDate = new Date(this.birth);
    let age = today.getFullYear() - birthDate.getFullYear();
    const m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
      age--;
    }
    return age;
  }

  public toString(): string {
    return `[Person ${this.getName()}]`;
  }
}
```

### ✅ 사용 예시
```typescript
const person = new Person("홍길동", "1990-01-01");
console.log(person.getName());
console.log(person.getBirth());
console.log(person.getAge());
console.log(person.toString());
```


## 📱 Phone 클래스 (JavaScript)
```javascript
class Phone {
  constructor(name) {
    this.name = name;
  }

  setName(newName) {
    this.name = newName;
  }

  getName() {
    return this.name;
  }

  toString() {
    return `[Phone ${this.getName()}]`;
  }
}
```
## 📱 Phone 클래스 (TypeScript)
```typescript
class Phone {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  public setName(newName: string): void {
    this.name = newName;
  }

  public getName(): string {
    return this.name;
  }

  public toString(): string {
    return `[Phone ${this.getName()}]`;
  }
}
```

## 🔍 비교 요약
| 항목             | JavaScript 클래스             | TypeScript 클래스                   |
|------------------|-------------------------------|-------------------------------------|
| 타입 명시        | 없음                          | 명확한 타입 지정 (`string`, `number`) |
| 접근 제어        | 제한 없음                     | `private`, `public`으로 캡슐화 강화 |
| 안정성           | 런타임 오류 가능              | 컴파일 시점에 오류 탐지 가능        |
| 캡슐화 수준      | 기본적인 구조                 | 명시적 접근 제어로 보안성 향상      |


# new

JavaScript에서 new 키워드를 사용할 때 프로토타입 함수들이 객체에 어떻게 연결되는지는 객체지향의 핵심 메커니즘 중 하나입니다.  
아래에 그 과정을 단계별로 설명.

## 🔧 new 키워드의 동작 메커니즘
```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.sayHello = function() {
  console.log("Hello, " + this.name);
};

const p = new Person("JungHwan");
```

## 🧠 내부 동작 순서
- 빈 객체 생성
  - new Person()을 호출하면 JavaScript는 먼저 **빈 객체 {}** 를 생성합니다.
- 프로토타입 연결
  - 생성된 객체는 Person.prototype을 자신의 [[Prototype]] (또는 __proto__)에 자동으로 연결합니다.
  - 즉, p.__proto__ === Person.prototype이 됩니다.
- 생성자 함수 실행
  - Person 함수가 호출되며, this는 새로 생성된 객체를 가리킵니다.
  - this.name = name이 실행되어 p.name = "JungHwan"이 됩니다.
- 객체 반환
  - 생성자 함수가 명시적으로 객체를 반환하지 않으면, new는 자동으로 this (즉, 새 객체)를 반환합니다.

## 🔗 프로토타입 함수 연결 구조
```javascript
console.log(p.sayHello); // Person.prototype.sayHello 참조
p.sayHello();            // Hello, JungHwan
```
- sayHello는 p 객체에 직접 정의된 것이 아니라, Person.prototype에 정의된 함수입니다.
- p는 자신의 프로토타입 체인을 따라 sayHello를 찾습니다 → 프로토타입 기반 상속

## 📦 시각적 구조
```
Person.prototype
   └── sayHello()

p (new Person)
   ├── name: "JungHwan"
   └── [[Prototype]] → Person.prototype

```

## ✅ 요약

| 단계/개념            | 설명                                                                 |
|----------------------|----------------------------------------------------------------------|
| new                  | 생성자 함수를 호출하여 새로운 객체 인스턴스를 생성                   |
| [[Prototype]].prototype | 새로 생성된 객체의 내부 [[Prototype]]이 생성자 함수의 prototype을 참조 |
| this                 | 생성자 함수 실행 시 새로 만들어진 객체를 가리키도록 바인딩됨         |
| 반환                 | 생성자 함수가 명시적으로 객체를 반환하지 않으면 `this`가 반환됨      |

---



