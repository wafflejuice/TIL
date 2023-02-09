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

# 2023-02-09
## schema vs. catalog
schema : 다음과 같은 정보가 정의되어 있다.

- 데이터베이스를 구성하는 테이블 정보(이름, 필드, 데이터 타입 등)와 테이블 간 관계(relationship) 같은 정보
- 데이터 조작 시 데이터 값들이 갖는 논리적인 제약 조건(constraints)
NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, DEFAULT, INDEX 등

catalog

데이터베이스 시스템 내의 모든 객체에 대한 정의와 명세입니다. 데이터 사전(data-dictionary)라고 불리기도 하며, 카탈로그에 저장된 내용을 메타-데이터라고 합니다. DDL( 실행 결과로 생성되는 기본 테이블, 뷰 테이블, 동의어, 값 범위, 인덱스, 사용자, 사용자 그룹, 스키마 정보 등이 저장됩니다. 데이터베이스 종류에 따라 다른 구조를 가지며, DBMS에 의해 스스로 생성되고 유지됩니다.

시스템 카탈로그는 시스템 테이블로 구성되어 있기 때문에 일반 사용자도 SQL 질의를 통해 내용을 검색할 수 있습니다. 하지만, INSERT, DELETE, UPDATE 문으로 카탈로그 정보를 직접 변경하는 것은 불가능합니다. 사용자가 DDL을 통해 테이블 뷰, 인덱스 등에 변화를 주면 시스템에 의해 자동으로 갱신됩니다.

스키마와 카탈로그의 차이점
- 카탈로그는 모든 종류의 스키마와 관련된 연결 고리들을 지닌 공간이다.
- 카탈로그는 시스템에 관심이 있는 다양한 개체들에 대한 자세한 정보들을 포함하고 있다.
- 카탈로그는 SQL 환경의 스키마들에 대한 묶음이다.
- 카탈로그는 한 개 이상의 스키마를 가질 수 있으며 INFORMATION_SCHEMA라는 이름의 스키마는 항상 가지고 있다.
- 카탈로그는 보통 데이터베이스의 동의어로 사용됩니다.

즉, catalog는 여러 개의 schema를 지닐 수 있으므로 schema보다 catalog가 상위 개념이다.

reference : https://junhyunny.github.io/database/database-schema-and-catalog/
