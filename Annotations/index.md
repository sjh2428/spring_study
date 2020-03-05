## @Bean @Configuration @Autowired @Resource @Inject @Qualifier @Primary @Component

`org.springframework.context.annotation` 패키지에 들어있는 `@Configuration`, `@Bean` 어노테이션을 기반으로 스프링 컨테이너 설정 파일임을 암시하고(`@Configuration`), 메소드를 스프링 컨테이너에 등록(`@Bean`)할 수 있다. 이렇게 등록된 bean은 스프링 컨테이너에서 주입받아 사용하게 된다.
<br>

### **핵심**

- `빈` 스프링에서 빈은 스프링 컨테이너에 의해서 관리되는 객체(Spring bean == Java Object)<br>
- `@Configuration` 어노테이션은 클래스 앞에 붙임으로써 `해당 클래스는 스프링 빈을 설정하는 클래스이다` 라는 것을 암시한다.
- `@Bean` 어노테이션은 메소드 상단에 적으며 default로 메소드 이름이 bean의 이름이 된다. 또한 이 메소드가 반환하는 객체가 bean이 되며 반드시 bean instance를 반환해야 한다.
- `@Component` 어노테이션은 클래스 상단에 적으며 default로 클래스 이름이 bean의 이름이 된다. 또한 Spring에서 자동으로 찾고(`@ComponentScan`을 사용하여) 관리해주는 bean이다.
- 객체 주입에는 `@Autowired`, `@Resource`, `@Inject` 어노테이션이 있다.
- TODO: ComponentScan 동작 원리

  - `@Autowired`

    - `byType`으로 객체를 주입하는데, 찾지 못하면 `byName`으로 주입
    - 스프링 프레임워크에 종속적
    - 프로퍼티나 메소드, 생성자에 사용 가능

  - `@Resource`

    - `byName`으로 객체를 주입하는데, 찾지 못하면 `byType`
    - 자바에서 제공하므로 프레임워크에 종속되지 않음
    - 생성자에 불가
    - 프로퍼티나 메소드에 사용 가능

  - `@Inject`
    - `@Autowired`와 차이점이라면 `@Autowired`의 경우 `required` 속성을 이용해서 의존 대상 객체가 없어도 `Exception`을 피할 수 있는 옵션을 제공하지만 `@Inject`의 경우 `required` 속성을 지원하지 않음

- bean의 scope - bean이 호출될 때 bean의 scope에 따라서 알맞은 bean 객체를 제공
  - Singleton (Default) - 매번 `동일한` 객체를 제공
  - Prototype - 매번 `다른` 객체를 제공

<br>

### **XML/Java 빈 설정 방법**

- 기존 `XML`을 활용하여 스프링 빈을 설정하던 방식

```xml
<bean id="userDao" class="me.dao.UserDao"/> <!-- 1 -->
<bean id="registerService" class="me.service.UserRegisterService"> <!-- 2 -->
  <constructor-arg ref="userDao" />
</bean>
<bean id="dbConnectionInfo" class="me.DbConnectionInfo"> <!-- 3 -->
  <property name="jdbcUrl" value="jdbc::oracle:url" />
  <property name="userId" value="aaaa" />
  <property name="userPw" value="bbbb" />
</bean>
```

- 자바 코드로 스프링 빈을 설정하는 방법

```java
@Configuration
public class UserConfig {

  @Bean
  public UserDao userDao() { // <!-- 1 -->
    return new UserDao();
  }

  @Bean
  public UserRegisterService registerService() { // <!-- 2 -->
    return new UserRegisterService(userDao());
  }

  @Bean
  public DbConnectionInfo dbConnectionInfo() { { // <!-- 3 -->
    UserDbConnectionInfo udbcInfo = new DbConnectionInfo();
    udbcInfo.setJdbcUrl("jdbc::oracle:url");
    udbcInfo.userId("aaaa");
    udbcInfo.userPw("bbbb");
    return udbcInfo;
  }
}
```

### **@Qualifier vs @Primary**

- 스프링은 default가 Singleton 방식으로 빈을 생성하는데, 프로토타입의 경우? or 같은 타입이 여러개일 경우?<br>
  `@Qualifier` or `@Primary` 사용<br>
  둘 다 존재하는 상황에서는 `@Qualifier`가 우선권을 가진다.

```java
@Bean(name = "hihi")
// ...

@Bean(name = "hoho")
// ...
```

```java
@Autowired
@Qualifier("hihi")
Say say1

@Autowired
@Qualifier("hoho")
Say say2
```

### @Component vs @Bean

- 둘의 차이는 [핵심](#핵심)에 적어 놓았고 코드로 다시 한번 보고자 함

```java
@Component
public class TestBean { ... }
```

```java
public class TestBean { ... }

@Configuration
public class TestConfig {
  @Bean
  public TestBean testBean() {
    return new TestBean();
  }
}
```
