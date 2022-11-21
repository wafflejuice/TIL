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
