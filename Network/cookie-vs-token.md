# Cookie Based Auth vs. Token Based Auth

## Cookie Based Authentication

1. 유저가 로그인 정보(아이디, 비밀번호)를 입력하여 서버에 던점.
2. 로그인 정보(credentials)가 유효함? 유효하면 세션을 만들고 DB에 저장. 세션 ID를 클라이언트에게 던짐.
3. 클라이언트의 브라우저의 쿠키에 세션ID 를 저장함.
4. 클라이언트는 매 request 마다 쿠키에 저장해 두었던 세션ID를 함께 담아 던짐.
5. 서버는 날라온 세션ID를 DB에 저장해 둔 세션에 대해 비교하여 유효성을 검사하고 request를 처리함.
6. 유저가 앱에서 로그아웃하면 클라이언트와 서버사이드 양측에서 세션이 삭제됨.

- stateful
- 인증 기록과 세션이 서버와 클라이언트 양측 모두에서 저장되어야 함
- 서버는 살아있는 세션을 DB에서 지속적으로 track 함
- 클라이언트는 세션ID를 저장함

## Token Based Authentication

1. 유저가 로그인 정보(아이디, 비밀번호)를 입력하여 서버에 던점.
2. 로그인 정보가 유요하면 토큰을 만들어서 클라이언트에게 던짐.
3. 클라이언트는 날아온 토큰을 저장함. 보통 local storage에 저장하지만 세션 스토리지나 쿠키에 저장할 수 도 있음.
4. 클라이언트는 매 request 마다 authorization header에 토큰을 담아서 ( 또는 위에서 언급한 다른방식을 이용하여 ) 날림.
5. 서버는 날아온 JWT를 해독(decode) 하여 유효성을 검사하고 request를 처리함.
6. 유저가 앱에서 로그아웃하면 클라이언트에서 토큰이 사라짐. 서버와의 추가 네트워킹이 필요 없음.

- stateless
- 서버는 어떤 유저가 로그인되어있고 어떤 토큰이 발급되어있는지 기록하지 않음.
- 매 request에 딸려온 토큰을 이용해서 인증의 유효성을 검사함

참조: https://auth0.com/blog/cookies-vs-tokens-definitive-guide/
