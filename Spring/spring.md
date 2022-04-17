# 2022-04-17
## spring container is singleton container
- spring container는 객체 instance를 singleton으로 관리한다.
- 이렇게 singleton 객체를 생성하고 관리하는 기능을 singleton registry라고 한다.
- spring container는 singleton pattern의 단점을 해결하였다.
    - spring이 관리해주기 때문에 DIP, OCP, 테스트, private constructor로부터 자유롭게 singleton을 사용할 수 있다.
    - @Configuration decorator를 붙이면 CGLIB이라는 바이트코드 조작 라이브러리를 사용하여 내가 등록하고자 하는 클래스를 상속받은 임의의 다른 클래스를 만들고, 그것을 스프링으로 등록하여 singleton을 보장하는 것이다.
***
