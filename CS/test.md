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
