# 2023-09-09
## JDBC
JDBC(Java Database Connectivity): 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API

# 2023-09-27
## JdbcTemplate Batch Insert: rewriteBatchedStatements should be turned-on for MySQL DB
MySQL DB 사용 시 `rewriteBatchedStatements` 옵션을 켜지 않으면 JdbcTemplate.batchUpdate하더라도 단건으로 요청이 나간다.
