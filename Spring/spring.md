# 2022-04-17
## spring container is singleton container
- spring container는 객체 instance를 singleton으로 관리한다.
- 이렇게 singleton 객체를 생성하고 관리하는 기능을 singleton registry라고 한다.
- spring container는 singleton pattern의 단점을 해결하였다.
    - spring이 관리해주기 때문에 DIP, OCP, 테스트, private constructor로부터 자유롭게 singleton을 사용할 수 있다.
    - @Configuration decorator를 붙이면 CGLIB이라는 바이트코드 조작 라이브러리를 사용하여 내가 등록하고자 하는 클래스를 상속받은 임의의 다른 클래스를 만들고, 그것을 스프링으로 등록하여 singleton을 보장하는 것이다.
***

# 2022-04-21
## Bean duplication
중복되는 Bean이 존재할 경우 해결책:
- 구체 클래스 사용 **(BAD)** (DIP 위반)
- @Autowired 필드명 매칭
```java
@Component
public class RateDiscountPolicy implements DiscountPolicy {
    ...
}

@Component
public class FixDiscountPolicy implements DiscountPolicy {
    ...
}

@Autowired
public class OrderServiceImpl {
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy rateDiscountPolicy) {
        ...
    }
}
```
- @Qualifier
```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {
    ...
}

@Component
@Qualifier("subDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {
    ...
}

@Autowired
public class OrderServiceImpl {
    public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
        ...
    }
}
```
- @Primary
```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {
    ...
}

@Component
public class FixDiscountPolicy implements DiscountPolicy {
    ...
}

@Autowired
public class OrderServiceImpl {
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        ...
    }
}
```

# 2022-04-22
## Lifecycle Callbacks
Java의 constructor, destructor와 비슷한 개념이 Spring에도 존재한다.  
Bean이 등록된 뒤(생성, DI가 끝난 상태)에 실행되는 초기화 콜백과, 빈이 소멸되기 직전에 호출되는 소멸전 콜백이 그것이다.  
[Spring document](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-nature)에서는 `@PostConstruct`, `@PreDestroy` annotator를 붙여 지정하는 것을 권장한다.  
외부 library를 사용하는 경우에는 다음과 같이 configuration에서 등록할 때 method name으로 지정한다.
```java
@Bean(initMethod = "init", destroyMethod = "close")
```

***

## ObjectProvider
singleton bean 내부에서 prototype bean을 사용하는 경우는, 그 이름이 암시하듯이 prototype bean을 spring container에 요청할 때마다 새로운 prototype bean을 받기를 원할 때이다. 단순히 singleton bean DI 시점에 prototype bean을 injection받으면 동일한 prototype bean을 유지하게 되므로 이 목적이 달성되지 않는다.  
DL(Dependency Lookup) 기능을 사용하여 해결한다. 필요할 때마다 spring container에서 찾는 것. `ApplicationContext`로 조회하는 것도 가능하지만 `ObjectProvider`를 사용하면 간결하다.
```java
// default : @Scope("singleton")
static class ClientBean {

    private final ObjectProvider<PrototypeBean> prototypeBeanObjectProvider;
    
    @Autowired
    public ClientBean(ObjectProvider<PrototypeBean> prototypeBeanObjectProvider) {
        this.prototypeBeanObjectProvider = prototypeBeanObjectProvider;
    }

    public int logic() {
        PrototypeBean prototypeBean = prototypeBeanObjectProvider.getObject();
        ...
    }
}

@Scope("prototype")
static class PrototypeBean {
    ...
}
```
ObjectProvider는 request scope bean을 다룰 때에도 유용하게 쓰인다. singleton scope bean **S**가 request scope bean **R**을 가질 때, S의 생성 및 DI 시점에는 HTTP request가 들어오지 않았다. 이후 HTTP request가 들어오면 `ObjectProvider.getObject()`를 호출하여 lazy하게 **R**을 생성하는 식이다.

# 2022-04-23
## Proxy
request scope bean을 다룰 때(그리고 그외 필요할 상황에서) ObjectProvider보다 더 간편한 방법은 Proxy를 사용하는 것이다.
```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyLogger {
    ...
}

@Controller
@RequiredArgsConstructor
public class LogDemoController {
    private final MyLogger myLogger;
    ...
}
```
CGLIB이 내가 등록할 객체 **R**을 상속하는 proxy 객체 **P**를 생성하고, spring container에 **P**를 등록한다. **P**에는 요청이 오면 그때 **R**을 요청하는 위임 로직이 들어있다.  
따라서 사용자 입장에서는 HTTP request가 들어올 때 **P**를 요청하면, 그때 **P**가 **R**을 요청하므로 원하는 결과를 얻게 된다. 마치 singleton bean처럼 사용할 수 있는 것이다.

***

## @RequestMapping, @GetMapping, @PostMapping
@GetMapping, @PostMapping을 @RequestMapping의 shortcut으로 사용할 수 있다.  
`@RequestMapping(value = "/...", method = RequestMethod.GET)` can be replaced by `@GetMapping("/...")`  
`@RequestMapping(value = "/...", method = RequestMethod.POST)` can be replaced by `@PostMapping("/...")`  

# 2022-05-11
## @Transactional
@Transactional annotation은 @Test와 함께 사용될 경우 Test가 끝나면 DB를 rollback한다. 직접 DB를 확인해보고 싶다면 @Rollback(false) annotation을 사용한다.

# 2022-05-12
## Spring Data JPA
1:N relation
```java
// N-side
@ManyToOne
@JoinColumn(name = "column_name")

// 1-side
@OneToMany(mappedBy = "entity_member_name")
```

1:1 relation에서는 조회가 더 빈번한 쪽을 FK로 삼는다.
```java
// frequent
@OneToOne
@JoinColumn(name = "column_name")

// rare
@OneToOne(mappedBy = "entity_member_name")
```

## Spring Data JPA, EnumType
Enum annotation으로 EnumType.ORDINAL(default)과 EnumType.STRING이 존재한다. ORDINAL은 중간에 다른 enum type이 추가되면 그 뒤 enum type의 값들이 한 칸씩 밀리기 때문에 쓰면 안 된다.
```java
@Enumerated(EnumType.STRING)
private DeliveryStatus status; // READY, COMP
```
