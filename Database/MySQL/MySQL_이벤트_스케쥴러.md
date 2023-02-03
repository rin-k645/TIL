# MySQL 이벤트 스케쥴러

프로젝트를 진행하던 중, 게시글을 등록한지 30분이 지나면 자동으로 게시글이 삭제가 되는 기능이 필요했다. DB에서 일정시간이 지나면 자동으로 업데이트를 해주는 기능으로 '이벤트 스케줄러'가 생각이 났다!

<br>
<br>
<br>

## 1. 이벤트 스케줄러 설정 확인

<br>

```SQL
SHOW VARIABLES LIKE 'event%';
```

Value가 ON 이면, 스케쥴러 설정이 가능하다.

<br>
<br>
만약 OFF 라면, 아래의 쿼리를 통해 변경이 가능하다.
<br>
<br>

```SQL
SET GLOBAL event_scheduler = ON;
```

<br>
<br>
<br>

## 2. 이벤트 스케줄러 생성

<br>