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

# 2022-07-11
## collection vs. store
컬렉션 (collection)  
- POST 기반 등록
- 서버가 리소스 URI 결정

스토어 (store)  
- PUT 기반 등록
- 클라이언트가 리소스 URI 결정

주로 컬렉션을 사용한다.

## control URI
control URI : HTTP 메서드로 해결이 어려울 경우 사용한다. (동사 사용)  
ex) HTML form은 GET, POST만 지원한다.  
ex) /members/{id}/**new**  

# 2022-08-24
## HTTP status code
- 1xx (Informational): 요청이 수신되어 처리중
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행동 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못 함
