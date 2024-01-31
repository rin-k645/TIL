블로그 : https://programmingiraffe.tistory.com/168

<br>
<br>
<br>

## 1. Redis란?

`in-memory` 기반의 `NoSQL`로, `key-value`의 데이터 구조를 사용하는 데이터베이스

<br>

### NoSQL

- Not Only SQL’로 ‘SQL만을 사용하지 않는 데이터베이스’라는 뜻으로 기존의 RDBMS와는 다른 구조로 데이터를 저장한다.
- DBMS(관계형 데이터베이스)는 데이터 간 관계를 갖는 테이블 구조로 데이터를 저장하고, SQL을 통해 데이터에 접근한다.
- NoSQL은 Document, key-value 등의 **비관계형 데이터**를 저장하고, 별도의 SQL 대신 명령어로 데이터에 접근한다.

<br>

### in-memory

- 데이터를 디스크가 아니라, **메모리에 저장**하는 데이터베이스
- 디스크에 데이터를 저장하게 되면, 별도의 I/O(입출력) 작업을 수행하기 때문에 데이터를 읽고 쓰는 데 시간이 걸리게 된다.
- 메모리에 데이터를 저장하게 되면, 바로 메모리에 접근하기 때문에 **데이터 처리 속도가 빠르다**.
- 메모리가 휘발성이 있기 때문에 데이터가 유실될 수 있다.

<br>

NoSQL과 in-memory는 완전히 같은 개념이 아니고, in-memory는 NoSQL의 방식 중 하나로 NoSQL이 더 큰 개념이다.

<br>

### key-value

- NoSQL은 Document, Key-value, Graph 등 다양한 형태의 데이터 구조를 지원한다.
- Redis는 그 중에서 key-value 형태의 데이터 구조를 지원한다.

- Java에서 자주 쓰이는 자료구조 Hash와 데이터 구조인 JSON, XML을 떠올리면 된다.

<br>
<br>
<br>

## 2. Redis의 특징

- 영속성을 지원하는 인메모리 데이터 저장소
  - 데이터의 빠른 접근을 위해 인메모리 저장소를 씁니다. 하지만 데이터의 영속성을 보장하기 위해 디스크에 저장.
  - 서버에 문제가 발생해도 디스크에 저장된 데이터를 통해 복구가 가능.
- **다양한 데이터 타입**을 지원
  - String, List, Hash, Set, Sorted Set과 같은 다양한 자료구조를 지원.
- **Pub/Sub, TTL(Time To Live)와 같은 다양한 기능** 제공
  - 메시지를 게시하고, 구독자에게 전달하는 시스템을 구축할 수 있다.
  - 특정 기간 뒤에 데이터를 삭제하도록 할 수 있다.
- 분산환경에서 안정적인 데이터 관리 가능
- 명령어를 싱글 스레드로 하나씩 처리
- C, Java, Python 등의 다양한 언어 환경에서 개발 가능

<br>
<br>
<br>

## 3. Redis의 활용

- 캐싱
- 실시간 순위표
- 채팅
- 세션 관리

<br>
<br>
<br>

## 참고

- https://aws.amazon.com/ko/elasticache/what-is-redis/
