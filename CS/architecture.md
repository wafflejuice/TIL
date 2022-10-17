# 2022-10-12
## package composition - layered architecture
<만들면서 배우는 클린 아키텍처>(Tom Hombergs) pp.24-25

계층적 구성
```
buckpal
  - domain
    - Account
    - Activity
    - AccountRepository
    - AccountService
  - persistence
    - AccountRepositoryImpl
  - web
    - AccountController
```

Cons
- 애플리케이션의 기능 조각(functional slice)이나 특성(feature)을 구분 짓는 package 경계가 없다.
- 애플리케이션이 어떤 use-case들을 제공하는지 파악할 수 없다.
- 목표로 하는 아키텍처를 파악할 수 없다.

# 2022-10-13
## package composition - functional architecture
<만들면서 배우는 클린 아키텍처>(Tom Hombergs) pp.25-26

기능으로 구성
```
buckpal
  - account
    - Account
    - AccountController
    - AccountRepository
    - AccountRepositoryImpl
    - SendMoneyService
```

Pros
- package-private 접근 수준을 이용해 package 간 경계를 강화하여 불필요한 의존성 방지 가능

Cons
- 가시성이 훨씬 떨어진다

# 2022-10-17
## input and output should be specific among usecases
<만들면서 배우는 클린 아키텍처>(Tom Hombergs) pp.49-50

입력과 비슷하게 출력도 각 usecase마다 **구체적**일수록 좋다. 같은 출력 모델을 공유하면 usecase들도 강하게 결합되기 때문이다.
같은 이유로, domain entity를 출력 모델로 사용해서도 안 된다.
