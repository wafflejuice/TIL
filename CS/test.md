# 2022-07-07
## FIRST principles
- Fast
- Independent
- Repeatable
- Self-validating
- Timely

## SpringBoot test vs. Mock test
SpringBoot test
- Context 로딩이 오래 걸린다.  
- 데이터나 상태 등이 테스트 간에 영향을 미친다.  
- 좀더 정밀한 테스트가 불가능하다.  

Mock test
- 실행속도가 빠르다.  
- 테스트 간의 영향이 적다.  
- 정밀한 테스트가 가능하다.  

사용 예시  
- http request 던질 경우 -> SpringBoot test  
- string util, date util 등을 테스트할 경우 -> mock test

# 2022-07-29
## 제어할 수 없는 값은 테스트를 어렵게 만든다.
Java + JUnit4를 다루는 ["스프링 부트와 AWS로 혼자 구현하는 웹 서비스" 책을 Kotlin + JUnit5로 변환해가며 공부](https://github.com/wafflejuice/web-service-tutorial-in-kotlin.git)하던 중 다음과 같은 code가 들어간 test를 작성하게 되었다.

```kotlin
val now = LocalDateTime.of(2019, 6, 4, 0, 0, 0)
```

DB에 저장된 Entity의 created time과 modified time이 정상적으로 기록되는지 확인하는 test이다.  
고정된 날짜는 시간이 지날수록 먼 과거가 될 것이고, 가장 가까운 날짜를 사용하는 편이 더 좋은 테스트일 수 있겠다는 생각에 다음과 같이 구현하였다.

```kotlin
val now = LocalDateTime.now()
```

우연찮게 그날 저자의 블로그를 보다가 [1. 테스트하기 좋은 코드 - 테스트하기 어려운 코드](https://jojoldu.tistory.com/674)라는 글을 읽었는데, 정확히 내가 작성한 코드가 나쁜 예시로 제시되어 있었다.  
간단한 예제였기 때문에 test는 통과했지만, 미처 생각하지 못 했던 로직이 들어있을 경우, 예를 들어 특정 요일를 체크하는 로직이 존재한다면 잘못된 테스트가 될 수 있었던 것이다.  
random value를 사용할 때는 seed를 고정하는 것이 좋다고 생각함에도 맥락이 살짝 달라지니 알아채지 못 했다.

# 2022-08-25
## code coverage & branch coverage
- code coverage : 전체 코드라인 중, 테스트에서 실행되는 코드라인의 비율
- branch coverage : 전체 분기 중, 테스트하는 분기의 비율
