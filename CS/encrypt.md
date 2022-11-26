# 2022-11-26
## claim 기반 토큰 방식
claim 기반 토큰 방식 : 토큰 자체가 (사용자 ID나 권한 등의) 정보를 가지는 방식.

토큰 생성 흐름
1. client -> authorization server : ID, PW, token 권한을 넘기며 토큰 요청
2. authorization server : 사용자 계정을 확인하고, 권한을 부여해도 되면 토큰에 관련 정보를 넣어 반환한다.

API 호출 흐름
1. API 호출 시 토큰을 함께 전송한다.
2. 토큰 내 사용자 정보로 권한 인가 처리 후 결과를 반환한다.

# 2022-11-24,26
## JWT
JWT (JSON Web Token)
- claim 기반 토큰 방식
- JSON 형태의 claim을 Base64로 인코딩한다.
- 변조 방지 : secret(대칭키)으로 메시지를 해싱한 값을 메시지 뒤에 붙이는 HMAC 방식으로 변조를 방지한다. (메시지를 변조할 경우, secret을 모르므로 올바른 해시를 생성할 수 없다.)
- 사용된 서명 알고리즘을 JSON으로 정의 후 Base64로 인코딩하여 앞에 붙인다.

형태는 다음과 같다.
```
Base64(signature algorithm JSON).Base64(claim JSON).hash(claim JSON)
```

- claim에 넣는 데이터가 많아질수록 JWT 토큰 길이가 길어진다는 단점이 존재한다. API 호출마다 헤더에 포함되어야 하므로 그만큼 네트워크 대역폭이 낭비된다.
- claim을 암호화하지 않으므로(단순히 Base64 인코딩) 토큰이 노출되면 토큰 내부 정보를 통해 사용자 정보가 누출될 가능성이 있다. (이를 방지하기 위해 토큰 자체를 암호화하는 방법인 JWE(JSON Web Encryption) 존재)
