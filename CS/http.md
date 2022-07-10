# 2022-07-10
## HTTP

HTTP-message
- start-line
- *( header-field CRLF )
- CRLF
- [ message-body ]

start-line = request-line / status-line  
request-line = method SP request-target SP HTTP-version CRLF  
status-line = HTTP-version SP status-code SP reason-phrase CRLF  

header-field = feild-name ":" OWS field-value OWS  

크게 성공하는 기술은 단순하지만 확장 가능한 기술

**안전 (Safe)**  
호출해도 리소스를 변경하지 않는다.  
- GET
- HEAD

**멱등 (Idempotent)**  
n번 호출해도 결과가 같다.  
- GET
- PUT
- DELETE

자동 복구 메커니즘에 활용 가능  
*멱등은 외부 요인으로 중간에 리소스가 변경되는 것까지는 고려하지 않는다.

**캐시 가능 (Cacheable)**  
응답 결과 리소스를 캐시해서 사용해도 되는가?  
- GET
- HEAD
- POST (△)
- PATCH (△)
