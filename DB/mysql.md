# 2022-11-20
## check timezone
```
SELECT @@time_zone;
```

# 2023-01-27
## EXPLAIN keyword
`EXPLAIN` keyword를 붙여 실행 계획 정보를 제공받을 수 있습니다.

EXPLAIN을 활용하여 개선한 slow query 사례)

### Conditions

PK
- i.id
- f.id

index on
- i.f_id
- f.p_id

### ASIS

Query
```SQL
EXPLAIN DELETE FROM i
    WHERE EXISTS (SELECT id FROM f
        WHERE i.f_id = f.id and f.p_id = 1234);
```

Result
select_type | table | key
------------|-------|-----
DELETE | i | null
DEPENDENT SUBQUERY | f | PRIMARY

### TOBE

Query
```SQL
EXPLAIN DELETE FROM i
    WHERE i.f_id IN (SELECT f_id FROM f
        WHERE i.f_id = f.id and f.p_id = 1234);
```

Result
select_type | table | key
------------|-------|-----
SIMPLE | f | p_id
DELETE | i | f_id

DEPENDENT prefix를 가지는 subquery는 외부 query에 의존적이므로 외부 query보다 먼저 실행될 수 없다 => 비효율적일 가능성이 높다.

왜 EXISTS subquery는 index를 활용하지 않고, IN subquery는 활용하는지는 아직 원인을 찾고 있습니다.

# 2023-02-08
## CHAR vs. VARCHAR vs. TEXT
CHAR
- 크기 <= 255 bytes (= 2^8 - 1)
- 고정 크기
- 빠른 검색 속도

VARCHAR
- 크기 <= 65535 characters after MySQL 5.0.3 (= 2^16 - 1)
- 크기 <= 65535 bytes before MySQL 5.0.3 (= 2^16 - 1)
- 가변 크기
- memory 저장 -> 빠른 검색 속도
- index 가능

TEXT
- 크기 = 65535 (= 2^16 - 1)
- 가변 크기
- disk 저장 -> 느린 검색 속도
- index 불가능
