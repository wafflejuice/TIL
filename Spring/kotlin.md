# 2022-07-26
## Kotlin with Spring boiler plate
```kotlin
@SpringBootApplication
open class Application // (1)

fun main(args: Array<String>) {
//    SpringApplication.run(Application.class, args)
    runApplication<Application>(*args) // (2)
}
```
- (1) : should be `open` (if `kotlin.allopen` plugin is not contained in dependencies)
- (2) : use `runApplication` instead of `SpringApplication.run` (in java)

## An persisten class needs an empty constructor for JPA
> The JPA specification requires that all persistent classes have a no-arg constructor.
[Apache OpenJPA doc](https://openjpa.apache.org/builds/1.2.3/apache-openjpa/docs/jpa_overview_pc.html)

secondary constructor를 만들어줘도 되지만 plugin으로 자동으로 생성되게 할 수 있다.

```kotlin
kotlin("plugin.jpa") version "1.6.20"
```

`@Entity`, `@Embeddable`, `@MappedSuperclass` annotation이 달린 class의 empty constructor를 생성해준다. [kotlin doc](https://kotlinlang.org/docs/no-arg-plugin.html#jpa-support)

## @After, @AfterEach, @AfterTest
- org.junit.After
- org.junit.jupiter.api.AfterEach
- kotlin.test.AfterEach

JUnit4's `@After` == JUnit5's `@AfterEach`
 [junit doc](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)  

`@AfterTest` is an alias of `@AfterEach`

```kotlin
// JetBrains/kotlin/libraries/kotlin.test/junit5/src/main/kotlin/Annotations.kt
public actual typealias AfterTest = org.junit.jupiter.api.AfterEach
```

## Requirements to use Kotlin with Spring Boot
> To use Kotlin, `org.jetbrains.kotlin:kotlin-stdlib` and `org.jetbrains.kotlin:kotlin-reflect` must be present on the classpath. [spring doc](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.kotlin.requirements)

## No-args compiler plugin
`kotlin-noarg` plugin generates a constructor with no arguments using reflection.
> The generated constructor is synthetic so it can’t be directly called from Java or Kotlin, but it can be called using reflection. [kotlin doc](https://kotlinlang.org/docs/no-arg-plugin.html)

`kotlin-jpa` contains `kotlin-noarg`.
> As with the kotlin-spring plugin wrapped on top of all-open, kotlin-jpa is wrapped on top of no-arg. The plugin specifies @Entity, @Embeddable, and @MappedSuperclass no-arg annotations automatically. [kotlin doc](https://kotlinlang.org/docs/no-arg-plugin.html#jpa-support)

## POJO
POJO : Plain Old Java Object
> 주로 특정 자바 모델이나 기능, 프레임워크 등을 따르지 않은 자바 오브젝트를 지칭하는 말로 사용되었다. 스프링 프레임워크는 POJO 방식의 프레임워크이다. [korean wiki](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object)

# 2022-07-28
## webEnvironment
`webEnvironment` parameter for `@SpringBootTest` is `SpringBootTest.MOCK` at default.
```java
	/**
	 * The type of web environment to create when applicable. Defaults to
	 * {@link WebEnvironment#MOCK}.
	 * @return the type of web environment
	 */
	WebEnvironment webEnvironment() default WebEnvironment.MOCK;
```

`SpringBootTest.WebEnvironment` 사용하면 내장 서버를 랜덤 포트로 띄운다.  
이후 `TestRestTemplate`를 주입받아 사용한다.

```kotlin
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
internal class PostsApiControllerTest(
    @LocalServerPort
    private val port: Int,

    @Autowired
    private val restTemplate: TestRestTemplate,

    @Autowired
    private val postsRepository: PostsRepository
) {
    ...
}
```

## @Id type vs. type?
```kotlin
@Entity
data class CompanyClass(
    @Id
    val companyCode: String,
)
```

```kotlin
@Entity
data class CompanyClass(
    @Id @GeneratedValue
    val companyCode: String? = null,
)
```

## createdDate, modifiedDate
```kotlin
@MappedSuperclass
@EntityListeners(AuditingEntityListener::class)
class BaseTimeEntity(
    @CreatedDate
    @Column(updatable = false)
    var createdDate: LocalDateTime = LocalDateTime.now(),

    @LastModifiedDate
    var modifiedDate: LocalDateTime = LocalDateTime.now()
)

@Entity
class myEntity(
    ...
): BaseTimeEntity() {
    ...
}
```

단, build에서 all open 설정을 하지 않았다면 open 키워드를 붙여줘야 한다.

```
// build.gradle.kts

allOpen {
    annotation("javax.persistence.MappedSuperclass")
}
```

or

```kotlin
@MappedSuperclass
@EntityListeners(AuditingEntityListener::class)
open class BaseTimeEntity(
    ...
```

# 2022-08-09
## an annotation as a parameter in kotlin
```kotlin
@WebMvcTest(
    HelloController::class,
    excludeFilters = [
        @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = [SecurityConfig::class])
    ]
)
```

`An annotation can't be used as the annotations argument` error occurs.

correct:
```kotlin
@WebMvcTest(
    HelloController::class,
    excludeFilters = [
        ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = [SecurityConfig::class])
    ]
)
```

> If an annotation is used as a parameter of another annotation, its name is not prefixed with the @ character:

```kotlin
annotation class ReplaceWith(val expression: String)

annotation class Deprecated(
        val message: String,
        val replaceWith: ReplaceWith = ReplaceWith(""))

@Deprecated("This function is deprecated, use === instead", ReplaceWith("this === other"))
```

[kotlin doc](https://kotlinlang.org/docs/annotations.html#constructors)

# 2022-08-12
SpringBoot에 포함된 ObjectMapper를 사용하면 kotlin에서는 error 발생.
> com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Cannot construct instance of `hello.survlet.basic.HelloData` (no Creators, like default constructor, exist): cannot deserialize from Object value (no delegate- or property-based Creator)

kotlin module인 `jacksonObjectMapper` 사용.
```kotlin
// val objectMapper: ObjectMapper = ObjectMapper() // error
val objectMapper: ObjectMapper = jacksonObjectMapper()
val helloData = objectMapper.readValue(messageBody, HelloData::class.java)
```

# 2022-08-20
## @Component DI
kotlin에서 `@Component` annotation이 붙은 class의 primary constructor는 자동으로 DI된다.

## TransactionTemplate.execute { ... }
`TransactionTemplate.execute { ... }`로 `@Transactional`보다 유연한 transaction 처리 가능

## @Value
properties에서 정의한 값 사용 가능.
```kotlin
@Value("${hello.spring}") hello
```

# 2022-10-16
## Bean Validation API
in java
```java
public class User {
    @Min(value = 19, message = "not adult")
    private final Int age;

    public User(Int age) {
        this.age = age;
    }
}
```
in kotlin
```kotlin
class User(
    @field:Min(value = 19, message = "not adult")
    private val age: Int
)
```
Change kotlin code into kotlin bytecode, then decompile it to find the reason.

# 2022-11-02
## Don't use a data class as an entity class
> Here we don’t use data classes with val properties because JPA is not designed to work with immutable classes or the methods generated automatically by data classes.  
[spring guides](https://github.com/spring-guides/tut-spring-boot-kotlin#persistence-with-jpa)

# 2022-12-08
## Bean validation
Use `@{validation}` for **class**, `@field:{validation}` for **data class**.
```kotlin
 class Item(
    var id: Long? = null,

    @NotBlank
    var itemName: String? = null,

    @Range(min = 1000, max = 1000000)
    @NotNull
    var price: Int? = null,

    @Max(9999)
    @NotNull
    var quantity: Int? = null
)
```
```kotlin
data class Item(
    var id: Long? = null,

    @field:NotBlank
    var itemName: String? = null,

    @field:Range(min = 1000, max = 1000000)
    @field:NotNull
    var price: Int? = null,

    @Max(9999)
    @field:NotNull
    var quantity: Int? = null
)
```

# 2023-09-24
## Use lambda as a anonymous class with a only method
example
```kotlin
override fun findById(memberId: String): Member {
    val sql = "select * from member where member_id = ?"
    return template.queryForObject(sql, memberRowMapper(), memberId)!!
}

private fun memberRowMapper(): RowMapper<Member> {
    return RowMapper<Member> { rs: ResultSet, rowNum: Int ->
        Member(
            memberId = rs.getString("member_id"),
            money = rs.getInt("money"),
        )
    }
}
```

RowMapper interface
```java
@FunctionalInterface
public interface RowMapper<T> {

	@Nullable
	T mapRow(ResultSet rs, int rowNum) throws SQLException;

}
```
