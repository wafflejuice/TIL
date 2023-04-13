# 2022-08-20
## JPA example
```kotlin
fun findByAccountIdAndCreatedAtAfterOrderByCreatedAtDesc(
    accountId: Long,
    agreedAtLimit: LocalDateTime
): List<TermsAgreement>
```
is meaning
```sql
// pseudo
SELECT * FROM terms_agreement_table
WHERE accountId = accountId AND createdAt > agreedAtLimit
ORDER BY createdAt DESC
```

# 2022-11-12
## ddl-auto vs. hbm2ddl.auto
`spring.jpa.hibernate.ddl-auto` is a shortcut of `spring.jpa.hibernate.hbm2ddl.auto`.

> DDL mode. This is actually a shortcut for the "hibernate.hbm2ddl.auto" property. Defaults to "create-drop" when using an embedded database and no schema manager was detected. Otherwise, defaults to "none".

source: [spring doc](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#spring.jpa.hibernate.ddl-auto)

# 2022-11-20
## 
`@CreatedDate`, `@LastModifiedDate` are not available for `ZonedDateTime`???

# 2023-04-13
## annotations not needed for query methods
1. `@Transactional`
`SimpleJpaRepository`를 상속하는 Repository는 기본적으로 transactional하다.
[Spring doc](https://docs.spring.io/spring-data/jpa/docs/3.0.3/reference/html/#transactions)

2. `@Modifying`은 `@Query`와 함께 사용되어야지만 의미가 있으며, 쿼리 메서드에도 적용되지 않는다.
[Spring doc](https://docs.spring.io/spring-data/data-jpa/docs/3.0.3/api/org/springframework/data/jpa/repository/Modifying.html)
