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