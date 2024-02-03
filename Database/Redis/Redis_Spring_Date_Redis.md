블로그 : https://programmingiraffe.tistory.com/171

<br>
<br>
<br>

Spring에서 지원하는 Spring Data Redis를 통해서 Redis의 DB와 연동할 수 있다. Spring Data Redis를 활용하면 설정과 Redis의 데이터에 접근하는 것을 손쉽게 할 수 있다.

<br>
<br>

### 개발 환경

- `Redis 3.0`
- `Spring Boot 3.2`
- `Java 17`
- `Rombok`

<br>
<br>
<br>

## 의존성 추가

Spring Initiaizer로 시작한다면 `Spring Data Redis(Access+Driver)`를 추가해주면 된다.

<br>

Gradle

```groovy
implementation 'org.springframework.boot:spring-boot-starter-data-redis'
```

`build.gradle` 의 `dependencies`에 `spring-boot-starter-data-redis`를 추가해준다.

<br>
<br>
<br>

## 호스트와 포트번호 설정

`application.properties`

```groovy
spring.data.redis.host=localhost
spring.data.redis.port=6379
```

<br>

`application.yml`

```groovy
spring:
  data:
    redis:
      host: localhost
      port: 6379
```

<aside>
⚠️ Spring Boot 3.2 버전에서는 기존의 `spring.redis.host` 가 Deprecated 되어서 쓸 수가 없다. `spring.data.redis.host` 로 적어줘야 한다!
</aside>

<br>
<br>

`host`와 `port`를 설정할 수 있다.

`password`나 `pool` 등도 추가로 설정할 수 있다.

<br>
<br>
<br>

## Configuration 추가

Spring Data Redis는 Redis DB와의 연결을 위해 자바 Redis 오픈소스 라이브러리인 **Lettuce**와 **Jedis**를 활용한다.

Lettuce는 비동기로 동작해 Jedis에 비해 성능이 좋기도 하고, **Lettuce 사용을 권장**한다고 한다.

```java
@Configuration
public class RedisConfig {

    @Value("${spring.data.redis.host}")
    private String host;

    @Value("${spring.data.redis.port}")
    private int port;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }
}
```

Redis의 설정 정보를 Configuration Class에서 주입한다. `application.yml` 에서 설정한 값이 들어가게 된다.

`RedisConnectionFactory`는 Redis DB와 연결하는 역할을 한다.

<br>
<br>
<br>

Spring Data Redis가 제공하는 Redis의 데이터에 접근하는 방법에는 RedisTemplate 과 RedisRepositories 가 있다.

<br>
<br>
<br>

# RedisTemplate

RedisTemplate 은 **Operations** 을 제공해 Redis의 데이터에 접근한다.

<br>

### Configuration에 추가

```java
@Configuration
public class RedisConfig {

    @Value("${spring.data.redis.host}")
    private String host;

    @Value("${spring.data.redis.port}")
    private int port;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }

    //RedisTemplate 사용을 위한 추가
    @Bean
    public RedisTemplate<?, ?> redisTemplate() {
        RedisTemplate<?, ?> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        return redisTemplate;
    }
}
```

Configuration Class에 `RedisTemplate`설정을 추가한다.

<br>
<br>
<br>

### **RedisTemplate** 사용

`RedisTemplate` 은 자료구조 별 `Operations` 를 제공한다. 그리고 구현된 메소드로 Redis 명령어를 수행하면 된다.

```java
@SpringBootTest
class RedisTemplateTests {
    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Test
    void redisTemplateString() {
        ValueOperations<String, String> valueOperations = redisTemplate.opsForValue();

        String key = "name";
        valueOperations.set(key, "giraffe");
        String value = valueOperations.get(key);
        Assertions.assertEquals(value, "giraffe");
    }
}
```

`ValueOperations` : String 제공 클래스

`ListOperations` : List 제공 클래스

`SetOperations` : Set 제공 클래스

`ZSetOperations` : Sorted Set 제공 클래스

`HashOperations` : Hash 제공 클래스

<br>
<br>
<br>

# **Redis Repositories**

Redis Repositories는 매핑된 **Domain Object를 Redis의 Hashes로 변환하여 저장**한다. Spring Data JPA에서 Repository를 사용하는 것과 비슷하다.

`Redis 2.8.0` 이상의 버전에서 사용할 수 있고, 트랜젝션을 지원하지 않으므로 **RedisTemplate**과 써야 한다.

<br>

## Entity

```java
@Getter
@RedisHash("people")
public class Person {

    @Id
    private String id;
    private String firstname;
    private String lastname;

    public Person(String firstname, String lastname) {
        this.firstname = firstname;
        this.lastname = lastname;
    }
}
```

`@RedisHash` 의 값이 **Set**의 **key**로 들어가고, value(member)에는 `@Id` 의 값이 들어가게 된다.

(id값을 따로 설정하지 않으면 null로, 랜덤값이 들어가게 된다.)

`member:id값` 은 **Hash**이고, **key, value**에는 **Entity의 필드와 값**이 들어가게 된다.

<br>
<br>
<br>

## Repository

```java
public interface PersonRepository extends CrudRepository<Person, String> {

}
```

기본적인 CRUD를 제공하는 `CrudRepository` 를 상속받는다.

<br>
<br>
<br>

## 사용

```java
@SpringBootTest
public class RedisRepositoryTest {
    @Autowired
    private PersonRepository personRepository;

    @Test
    void test() {
        Person person = new Person("Giraffe", "Kim");
        personRepository.save(person);

        Person person2 = new Person("turtle", "Kim");
        personRepository.save(person2);

        personRepository.findById(person.getId());
//        personRepository.delete(person);
    }
}
```

- `save()` : id가 null이라면 새로운 id를 생성하고, **key**가 `keyspace:id` (people:id값)패턴으로 저장되고, Redis의 **Hash로 객체**(Person)**가 저장**된다.
- `findById()` : `keyspace:id` 에 해당되는 객체를 반환한다.
- `count()` : `@RedisHash` 로 정의한 keyspace(people)에 저장된 Enitity(Person)의 개수를 가져온다.
- `delete()` : 해당 객체(person)을 삭제한다.

<br>
<br>
<br>
<br>
<br>

## 참고

- Spring Data Redis 공식 문서 : https://docs.spring.io/spring-data/redis/reference/
- Lettuce VS Jedis : https://akku-dev.tistory.com/105
- Spring Boot 에서 Redis 사용하기 : https://bcp0109.tistory.com/328
- Redis Repository 공식 문서 : https://docs.spring.io/spring-data/redis/reference/repositories.html
