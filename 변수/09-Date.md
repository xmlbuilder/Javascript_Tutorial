# Date
JavaScript의 Date 객체는 시간과 날짜를 다루는 핵심 도구입니다.  
생성 방식, 구성 요소 추출, 포맷 변환 등 다양한 기능을 제공합니다.  
아래에 주요 사용법과 메서드를 표로 정리했습니다.  

## ✅ Date 객체 생성 방법
```rust
Date.now();                      // 현재 시간의 timestamp (ms)
new Date();                      // 현재 날짜와 시간
new Date(timestamp);            // timestamp 기반 날짜 생성
new Date('YYYY/MM/DD HH:mm:ss') // 문자열 기반 날짜 생성
new Date(2024, 3, 19, 12, 3, 10) // 연,월,일,시,분,초 직접 지정 (월은 0부터 시작)
```


## 📊 주요 메서드 요약표
| 메서드                     | 설명                                      | 예시 결과 (2024/04/19 12:34:12.052 기준) |
|----------------------------|-------------------------------------------|-------------------------------------------|
| getFullYear()              | 연도 반환 (4자리)                         | 2024                                      |
| getMonth() + 1             | 월 반환 (0~11 → +1 필요)                  | 4                                         |
| getDate()                  | 일 반환 (1~31)                            | 19                                        |
| getDay()                   | 요일 반환 (0:일 ~ 6:토)                   | 5 (금요일)                                |
| getHours()                 | 시 반환 (0~23)                            | 12                                        |
| getMinutes()               | 분 반환 (0~59)                            | 34                                        |
| getSeconds()               | 초 반환 (0~59)                            | 12                                        |
| getMilliseconds()          | 밀리초 반환 (0~999)                       | 52                                        |
| getTime() / valueOf()      | timestamp 반환 (1970 이후 ms)             | 1713497376484                             |
| toLocaleString()           | 지역 포맷 전체 날짜/시간                  | 2024. 4. 19. 오후 12:34:12                |
| toLocaleDateString()       | 지역 포맷 날짜만                          | 2024. 4. 19.                              |
| toLocaleTimeString()       | 지역 포맷 시간만                          | 오후 12:34:12                             |
| toDateString()             | 영문 날짜 문자열                          | Fri Apr 19 2024                           |
| toTimeString()             | 시간 + 타임존 정보                        | 12:34:12 GMT+0900 (대한민국 표준시)       |



## ✅ UTC 관련 메서드
```javascript
date.getUTCFullYear();     // UTC 기준 연도
date.getUTCMonth();        // UTC 기준 월
date.getUTCDate();         // UTC 기준 일
date.getUTCHours();        // UTC 기준 시
date.getUTCMinutes();      // UTC 기준 분
date.getUTCSeconds();      // UTC 기준 초
date.getUTCMilliseconds(); // UTC 기준 밀리초
```
- UTC 메서드는 로컬 시간과 다를 수 있으며, 서버나 국제 시간 처리에 유용합니다.


## ✅ 기타 생성 및 설정 메서드
```javascript
Date.parse("YYYY-MM-DD");         // 문자열 → timestamp
Date.UTC(YYYY, MM, DD, hh, mm);   // UTC 기준 timestamp 생성
date.setFullYear(2024);           // 연도 설정
date.setTime(timestamp);          // timestamp로 시간 설정
```


## ✅ 주의사항
- new Date(2024, 3, 19) → 월은 0부터 시작하므로 3은 "4월"
- new Date('2024/04/19 12:34:12:52') → 마지막 :52는 밀리초로 해석됨
- getDay()는 요일 반환 (0: 일요일 ~ 6: 토요일)
- Date.now()는 현재 시간의 timestamp만 반환 (Date 객체 아님)


---


## 📘 JavaScript 날짜 계산 & 타임존 처리
## ✅ 1. 날짜 차이 계산
### 🔹 두 날짜 간 차이 (밀리초, 일, 시간 등)
```javascript
const d1 = new Date('2024-04-19');
const d2 = new Date('2024-05-01');

const diffMs = d2 - d1; // 밀리초 차이
const diffDays = diffMs / (1000 * 60 * 60 * 24); // 일 단위
console.log(diffDays); // 12
```
- 🔍 Date 객체끼리 뺄셈하면 밀리초 차이가 계산됨


## ✅ 2. 날짜 더하기 / 빼기
### 🔹 하루 더하기
```javascript
const date = new Date();
date.setDate(date.getDate() + 1); // 하루 추가
console.log(date);
```        

### 🔹 3개월 빼기
```javascript
const date = new Date();
date.setMonth(date.getMonth() - 3); // 3개월 감소
console.log(date);
```

- 🔍 setDate(), setMonth(), setFullYear() 등으로 직접 조작 가능
- 자동으로 월/일 오버플로우 처리됨 (예: 2월 30일 → 3월 2일)


## ✅ 3. 타임존 처리
### 🔹 로컬 vs UTC
```javascript
const date = new Date('2024-04-19T12:00:00Z'); // UTC 기준
console.log(date.toString());         // 로컬 시간 기준 출력
console.log(date.toUTCString());      // UTC 기준 출력
console.log(date.toLocaleString());   // 시스템 로케일 기준 출력
```

## ✅ Date 출력 메서드 비교

| 메서드           | 설명                            | 출력 예시 (KST 기준)                          |
|------------------|----------------------------------|-----------------------------------------------|
| toString()       | 로컬 시간 + 타임존 정보 포함     | Fri Apr 19 2024 12:34:12 GMT+0900 (KST)       |
| toUTCString()    | UTC 기준 문자열                  | Fri, 19 Apr 2024 03:34:12 GMT                 |
| toLocaleString() | 시스템 로케일 기준 포맷          | 2024. 4. 19. 오전 12:34:12                    |


### 🔹 타임존 오프셋 확인
```javascript
const offset = new Date().getTimezoneOffset(); // 분 단위
console.log(offset); // -540 (KST는 UTC+9 → -9*60)
```

- 🔍 getTimezoneOffset()은 UTC 기준에서 얼마나 차이 나는지를 분 단위로 반환
- 한국(KST)은 -540, 미국 동부(EST)는 300 등


### ✅ 날짜 계산 요약표
| 기능           | 방법                                | 설명                                 |
|----------------|-------------------------------------|--------------------------------------|
| 날짜 차이      | `date2 - date1`                     | 밀리초 차이 반환                     |
| 일 단위 차이   | `diffMs / (1000 * 60 * 60 * 24)`    | 일수 계산                            |
| 날짜 더하기    | `setDate(getDate() + N)`            | N일 추가                             |
| 월/연도 조정   | `setMonth()`, `setFullYear()`       | 자동 오버플로우 처리됨              |
| UTC 변환       | `toUTCString()`                     | UTC 기준 문자열 반환                 |
| 타임존 확인    | `getTimezoneOffset()`               | UTC 기준 분 차이 반환                |



## ✅ 실전 팁
- 날짜 계산 시 항상 타임존 기준을 명확히 할 것 (서버 vs 클라이언트)
- Date.parse()는 브라우저마다 다르게 해석될 수 있으므로 ISO 형식 권장 (YYYY-MM-DDTHH:mm:ssZ)
- 날짜 라이브러리 사용 시 dayjs, date-fns, luxon 등이 경량이고 강력함

---
