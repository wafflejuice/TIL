# 2022-10-12
## package composition
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
