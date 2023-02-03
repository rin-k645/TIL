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

```SQL
delimiter |
CREATE EVENT delete_live
    ON SCHEDULE EVERY 10 SECOND
	ON COMPLETION PRESERVE ENABLE
    COMMENT '등록 후 30분이 지나면 삭제'
	DO
    BEGIN
		delete m
        from live_member m join live l
        on m.live_id = l.live_id
		where TIMESTAMPDIFF(MINUTE , l.regist_time, curtime()) >= 30;
        
		delete from live
		where TIMESTAMPDIFF(MINUTE , regist_time, curtime()) >= 30;
	END |
delimiter ;
```

- CREATE EVENT [이벤트명]
- ON SCHEDULE EVERY [반복 주기]
- ON COMPLETION NOT PRESERVE ENABLE : 이벤트 수행 후 이벤트를 삭제 (NOT 없으면 삭제하지 않는다.)
- COMMENT '코멘트'
- DO : 반복할 동작
- BEGIN, END : 안에 여러 동작 수행 가능하다.(쿼리마다 ; 를 붙여줘야 한다.)

<br>

- delimiter : 서버가 ; 로된, 여러 문장을 하나의 문장으로 이해할 수 있도록하는 명령어


<br>
<br>
<br>

## 3. 이벤트 스케줄러 확인

<br>

```SQL
SELECT * FROM information_schema.events;
``` 

<br>
<br>
<br>

## 4. 이벤트 스케줄러 삭제

<br>

```SQL
DROP event [스케줄러 이름];
```

<br>
<br>
<br>
<br>
<br>
<br>

## 참고 자료
- Mysql 이벤트 스케쥴러 (Event Scheduler) : https://jungeunpyun.tistory.com/64
- [MySQL 이벤트스케줄러 생성 (Event Scheduler)] : https://resilient-923.tistory.com/131
- [MySQL] Delimiter 사용법 : https://hogu-programmer.tistory.com/23
