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
