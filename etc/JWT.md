# JWT(Json Web Token)

- Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token
- JWT는 토큰 자체를 정보로 사용하는 Self-Contained 방식으로 정보 전달
- 생성된 토큰은 HTTP 통신을 할 때 Authorization이라는 key의 value로 사용된다. value에는 Bearer을 붙인다.
````
{"Authorization": "Bearer 토큰 값"}
````
````
- 접근 주체(Principal): 보호된 대상에 접근하는 사용자
- 인증(Authenticate): 사용자 확인, 애플리케이션의 작업을 수행할 수 있는 주체 증명
- 인가(Authorize): 사용자가 접근할 수 있는 권한이 있는지 검사
- 권한: 인증된 주체가 애플리케이션의 동작을 수행할 수 있도록 허락되있는지 결정, 권한 승인이 필요한 부분으로 접근하려면 인증 과정을 통해 주체가 증명 되어야 함
````

## JWT 구조
- Header, Payload, Signature
- Json 형태인 각 부분은 Base64로 인코딩 되어 표현
- "." 구분자를 사용하여 구분

### Header
- typ,alg
- typ: 토큰의 타입을 지정 ex) JWT
- alg: 알고리즘 방식을 지정하며, 서명(Signature) 및 토큰 검증에 사용 ex) HS256(SHA256) 또는 RSA
````
{ "alg": "HS256", "typ": JWT }
````

### PayLoad
- 토큰에서 사용할 정보의 조각들인 클레임(Claim)이 담겨있다.
- 클레임은 총 3가지로 나누어지며, Json(Key/Value) 형태로 다수의 정보를 넣을 수 있다.
- 등록된 클레임(Registered Claim)
````
// 토큰 정보를 표현하기 위해 이미 정해진 종류의 데이터, 선택적 작성
iss: 토큰 발급자(issuer)
sub: 토큰 제목(subject)
aud: 토큰 대상자(audience)
exp: 토큰 만료 시간(expiration), NumericDate 형식 
nbf: 토큰 활성 날짜(not before)
iat: 토큰 발급 시간(issued at), 토큰 발급 이후의 경과 시간을 알 수 있음
jti: JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용하며, 일회용 토큰(Access Token) 등에 사용
````
- 공개 클레임(Public Claim)
- 비공개 클레임(Private Claim)

### Signature(서명)
- 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드

### ExceptionHandler
- servlet filter에서 exception이 발생했을 때, handler 예시
````
// Custom JWT based security filter
httpSecurity
        .addFilterBefore(authenticationTokenFilterBean(), UsernamePasswordAuthenticationFilter.class);

// Custom Exception Filter for filter
httpSecurity
        .addFilterBefore(exceptionHandlerFilterBean(), JwtAuthenticationTokenFilter.class);
````
````
public class ExceptionHandlerFilter extends OncePerRequestFilter {

    @Override
    public void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        try {
            filterChain.doFilter(request, response);

        } catch (JwtException exception) {
        	response.setStatus(HttpStatus.BAD_REQUEST.value());
        } catch (RuntimeException exception) {
        	response.setStatus(HttpStatus.INTERNAL_SERVER_ERROR.value());
        }
    }
}
````