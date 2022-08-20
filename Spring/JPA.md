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
