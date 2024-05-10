# MySQL의 DATE_FORMAT

`DATE_FORMAT(칼럼명, 형식)` : 날짜를 지정한 형식으로 출력을 한다.

<br>

- %Y : 년도 4자리로 출력 ex) `2024`
- %y : 년도 2자리로 출력 ex) `24`

 <br>

- %m: 월 2자리로 출력 ex) `01`
- %c: 월 원래대로 출력 ex) `1`

 <br>

- %M : 월 영문으로 출력 ex) `January`
- %b : 월 영문 축약형으로 출력 ex) `Jan`

 <br>

- %d : 일 2자리로 출력 ex) `01`
- %e : 일 2자리로 출력 ex) `1`

 <br>

- %T : hh:mm:ss 형식으로 출력 ex) `13:30:00`
- %H : 24시간 형식으로 출력 ex) `13`
- %i : 분으로 출력 ex) `30`
- %S : 초로 출력 ex) `00`

<br>
<br>
<br>

## 예시
DATETIME(`2024-05-10 23:11:00`)에서 DATE(`2024-05-10`)만 추출해서 출력하기

<br>

```SQL
SELECT DATE_FORMAT(DATETIME, "%Y-%m-%d") AS 날짜
FROM TABLE_NAME
```

