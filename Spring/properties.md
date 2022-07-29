# 2022-07-29
## 웹페이지 한글 깨지는 문제
mustache + SpringBoot 환경에서 서버 구동시 웹페이지에 표시되는 한글이 깨진다. (`TestRestTemplate`를 사용한 test는 pass)

해결법은 `application.properties`에 `server.servlet.encoding.force-response=true` 추가
