블로그 : https://programmingiraffe.tistory.com/170

<br>
<br>
<br>

Redis는 String, Set, List, Hash, Sorted Set 등의 자료구조를 지원한다. 그리고 각 자료구조에 맞는 명령어를 통해 데이터를 읽고 쓸 수 있다.

<br>

![출처 : https://forum.redis.com/t/about-redis-commands-data-structures/13](https://global.discourse-cdn.com/standard17/uploads/redis/optimized/1X/891f134890043da5b983e219c695464e1c4f5c8b_2_517x300.png)
출처 : https://forum.redis.com/t/about-redis-commands-data-structures/13

<br>
<br>
<br>

## Strings

- 문자열(숫자, 직렬화된 객체, 이진수 등)을 저장한다.
- 512MB까지 저장할 수 있다.
- [명령어 공식 문서](https://redis.io/commands/?group=string)

<br>
<br>

삽입

```code
SET name giraffe
MSET name2 turtle color green rating 10
```

조회

```code
GET name
MGET name2 color rating
```

삭제

```code
DEL name
```

카운터

```code
INCR rating
DECR rating

INCRBY rating 5
DECRBY rating 5
```

<br>
<br>
<br>

## 옵션

- `EX` (Expiration) : 키에 대한 만료 시간(유효 기간)을 초 단위로 설정
- `NX` (Not eXists) : 키가 존재하지 않을 때만 값을 설정(키가 이미 존재하면 명령어가 실패)
- `XX` (eXists) : 키가 이미 존재할 때만 값을 설정(키가 존재하지 않으면 명령어가 실패)

<br>

```code
SET name giraffe EX 7
SET name giraffe NX
SET name giraffe XX
```

<br>
<br>
<br>

## Lists

- 문자열을 리스트로 중복해서 저장할 수 있다.
- 앞뒤로 삽입과 삭제가 가능하다.
- [명령어 공식 문서](https://redis.io/commands/?group=list)

<br>
<br>

앞에 삽입/뒤에 삽입

```code
LPUSH animals giraffe
LPUSH animals tiger
RPUSH animals bear
```

n개를 제거

```code
LPOP animals 1
```

크기

```code
LLEN animals
```

정해진 범위만큼 조회

```code
LRANGE animals 0 1
LRANGE animals -2 -1 //끝에서 2번째~첫번째
```

인덱스의 원소 조회

```code
LINDEX animals 0
```

원소의 인덱스 조회

```code
LPOS animals bear
```

<br>
<br>
<br>

## Sets

- 중복이 없는 문자열을 멤버로 저장할 수 있다.
- 집합 관계를 나타낸다.
- [명령어 공식 문서](https://redis.io/commands/?group=set)

<br>
<br>

삽입

```code
SADD people1 Noel Gem
SADD people2 Riam Andy
```

삭제

```code
SREM people1 Noel
```

유니온

```code
SUNION people1 people2
```

존재 여부

```code
SISMEMBER people1 Noel
```

<br>
<br>
<br>

## Hashes

- key-value로 된 데이터를 저장할 수 있다.
- [명령어 공식 문서](https://redis.io/commands/?group=hash)

<br>
<br>

삽입

```code
HSET songs:1 title 'Wonderwall'
HSET songs:1 artist 'Oasis'
```

조회

```code
HGET songs:1 title
HGETALL songs:1
```

존재 여부

```code
HEXISTS songs:1 title
```

key 조회

```code
HKEYS songs:1
```

value 조회

```code
HVALS songs:1
```

삭제

```code
HDEL songs:1 artist
DEL songs:1
```

<br>
<br>
<br>

## Sorted Sets

- 문자열과 점수를 저장한다.
- 점수순(오름차순, 내림차순)으로 정렬된다. (점수가 동일한 경우는 문자열 사전순으로 정렬)
- [명령어 공식 문서](https://redis.io/commands/?group=sorted-set)

<br>
<br>

삽입

```code
ZADD books 7 "book1" 2 "book2" 9 "book3"
```

조회

```code
ZRANGE books 0 -1 //인덱스 0부터 마지막 원소까지
ZRANGE books 0 -1 withscores //점수까지 반환
```

삭제

```code
DEL books
```

<br>
<br>
<br>
<br>
<br>
<br>

## 참고

- 공식문서 : https://redis.io/docs/data-types/

- 유튜브 튜토리얼 : https://youtube.com/playlist?list=PL4cUxeGkcC9h3V2eqhi8rRdIDJshP-b4P&si=ZYbbe35piGY7Hv0n
