블로그 : https://programmingiraffe.tistory.com/177

<br>
<br>
<br>

## Lombok 이란?
- Lombok은 자동적으로 `getter`, `setter`, `toString()`, `생성자` 등을 생성해주는 자바 라이브러리이다.
- 코드를 작성할 때 클래스에 @어노테이션만 붙인다면, 컴파일하는 과정에서 알아서 해당 코드가 생성된다.

<br>

## @어노테이션 종류

### @Getter / @Setter
getter와 setter를 생성해준다.

 
```java
public String getName() {
	return name;
}

public void setName(String name) {
	this.name = name;
}
```

<br>

### @ToString
toString() 메소드를 생성해준다.
```java
@Override
public String toString() {
	return "Person{" + "name='" + name + '\'' + ", age=" + age + '}';
}
```

<br>

### @EqualsAndHashCode
equals 메소드와 hashCode 메소드를 생성해준다.

```java
@Override
public boolean equals(Object o) {
	if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Person person = (Person) o;
    return age == person.age && Objects.equals(name, person.name);
}

@Override
public int hashCode() {
	return Objects.hash(name, age);
}
```

<br>

### @NoArgsConstructor, @AllArgsConstructor
각각 빈 생성자, 필드가 모두 들어간 생성자를 생성해준다.

```java
//@NoArgsConstructor
public Person() {}

//@AllArgsConstructor
public Person(String name, int age) {
	this.name = name;
    this.age = age;
}
```

<br>

### @RequiredArgsConstructor
final이 붙거나 @NotNull이 붙은 필드의 생성자를 생성해준다.
```java
@Service
@RequiredArgsConstructor
public class PersonService {
    private final PersonRepository personRepository;
}
```

<br>

Spring 의존성 주입 방식 중 하나인 생성자 주입 방식을 대체한다.

```java
@Service
public class PersonService {
    private final PersonRepository personRepository;

    @Autowired
    public PersonService(PersonRepository personRepository) {
        this.personRepository = personRepository;
    }
}
```

<br>

### @Data
@Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredArgsConstructor 를 모두 포함한다.
```java
@Getter
@Setter
@ToString
@EqualsAndHashCode
@RequiredArgsConstructor
public class Person {
    private String name;
    private int age;
}
```

<br>

### @Builder
```java
Person.builder()
	.name("giraffe")
	.age("1")
	.build();
```

<br>

### @Log
```java
private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());
```

@Log4j 와 같은 로그도 쓸 수 있다.

<br>
<br>
<br>

## Lombok 장점
- 어노테이션만 쓰면 알아서 자동으로 메소드를 생성해주기 때문에 편하다.
- 반복적인 코드를 줄일 수 있다. 코드량이 줄어든다.

<br>
<br>
<br>

## Lombok 단점과 주의사항
- IDE에서 플러그인을 설치해야 지원된다. 만약 다른 개발환경에서 Lombok 플러그인을 설치하지 않았다면, Lombok으로 개발했던 코드는 Lombok을 인식하지 못해 에러가 난다.
 

- Rombok을 쓰게 되면, 코드가 생략되기 때문에 코드가 어떻게 작동되고 있는지 이해하기가 어렵다. 만약 toString, equals, hasCode와 같이 오버라이딩해서 쓰는 메소드에서 디폴트와 다르게 재정의 할 필요가 있는 경우도 있기 때문에 주의해야 한다. 
 

- @Data는 @RequiredArgsConstructor도 포함하는데, @RequiredArgsConstructor를 사용하면 생성자의 파라미터 순서가 정해져 있기 때문에 변경될 경우 불편하다. 그래서 보통 @Builder를 많이 쓴다.
 

- @ToString 같은 경우는 순환 참조의 원인이 될 수 있다.