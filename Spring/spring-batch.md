# 2023-09-19
## 팀에 공유한 Spring Batch 학습 내용
1. Spring Batch의 기본 구조 https://docs.spring.io/spring-batch/docs/current/reference/html/job.html#configureJob
    1. Job
    2. Step
    3. Tasklet
    4. JobParameters
    5. JobInstance
    6. Execution
    7. JobLauncher
    8. Listener
2. DB setting sql
    1. cmd+shift+a → schema-mysql.sql file
3. JobParameters를 넘기기 까다롭다
    1. LocalDate를 지원하지 않는다
        1. Date는 지원하지만, LocalDate는 지원하지 않는다 https://hodolman.tistory.com/17
        2. 시도한 방법: String으로 변환 & 변환해주는 객체 Injection https://jojoldu.tistory.com/490
        3. Spring Batch 5 도입으로 해결됨: LocalDate 지원
4. Spring Batch 5
[https://velog.io/@freddiey/JobLauncher사용시-RunIdIncrementer적용하기](https://velog.io/@freddiey/JobLauncher%EC%82%AC%EC%9A%A9%EC%8B%9C-RunIdIncrementer%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)
https://alwayspr.tistory.com/49
    1. Java 17 Requirement
    2. 다양한 JobParameters Type 지원
    3. 명시성
        1. JobBuilderFactory is deprecated.
        JobBuilder를 사용하며 JobRepository를 명시해야 한다.
        2. StepBuilderFactory is deprecated.
        StepBuilder를 사용하며 JobRepository를 명시해야 한다.
5. 작성한 코드의 구조
    1. dependency
    2. 배치 자동 실행 방지 [https://velog.io/@freddiey/JobLauncher사용시-RunIdIncrementer적용하기](https://velog.io/@freddiey/JobLauncher%EC%82%AC%EC%9A%A9%EC%8B%9C-RunIdIncrementer%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)
    3. @JobScope, @StepScope: late binding https://jojoldu.tistory.com/330
    4. IdIncrementer를 통한 동일 파라미터 중복 실행 허용 [https://velog.io/@freddiey/JobLauncher사용시-RunIdIncrementer적용하기](https://velog.io/@freddiey/JobLauncher%EC%82%AC%EC%9A%A9%EC%8B%9C-RunIdIncrementer%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)
    외부 trigger 시 JobExplorer 전달 필요
6. Further more
    1. Batch Test
    2. ItemReader, ItemProcessor, ItemWriter
    3. FlowJob로 분기 ( ≠ SimpleJob)
