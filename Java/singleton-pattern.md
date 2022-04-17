# 2022-04-17
## singleton pattern
NOT lazy example
```java
public class SingletonService {

    private static final SingletonService instance = new SingletonService();

    public static SingletonService getInstance() {
        return instance;
    }

    // A private constructor prevents someone from creating a new object.
    private SingletonService() {}
}
```

장점
- 객체 하나만 존재하는 것을 보장하므로
    - 매번 생성하는 것에 비해 속도가 빠르다.
    - 하나만 존재해야 할 객체가 여러 개 존재하는 것을 막을 수 있다.

단점
- DIP 위반 (구체 클래스에 의존)
- private constructor -> child class 만들기 어렵다.
- 유연성이 떨어진다.

**CAUTION**
- singleton have to be **stateless**, read-only

***
