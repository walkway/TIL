# REST API
- Representational safe transfer
- 느슨한 결함(Loosely coupling) 형태의 구조
- 리소스, 메서드, 메세지
- 컨텍스트 저장소에 상태 정보를 저장하지 않는 형태
- ex) 이름이 a 사용자를 생성한다. 호출
- "사용자"는 생성되는 리소스, "생성한다"라는 행위는 메서드 그리고 "이름이 a 사용자"는 메시지가 된다.
- REST 형태로 표현해보면 다음과 같은 형태를 가진다.
````
HTTP POST, http://myweb/users/
{  
   "users":{  
      "name":"a"
   }
}
````

## 6가지 제한 조건
- 클라이언트/서버 구조: 일관적인 인터페이스로 분리되어야 한다
- 무상태(Stateless): 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다
- 캐시 처리 가능(Cacheable): WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 한다.
 - 잘 관리되는 캐싱은 클라이언트-서버 간 상호작용을 부분적으로 또는 완전하게 제거하여 scalability와 성능을 향상시킨다.
- 계층화(Layered System): 클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지를 알 수 없다. 중간 서버는 로드 밸런싱 기능이나 공유 캐시 기능을 제공함으로써 시스템 규모 확장성을 향상시키는 데 유용하다.
- Code on demand (optional) - 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.
인터페이스 일관성: 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다.

## PATCH vs PUT
### PUT
- 자원의 전체 교체, 자원교체 시 모든 필드 필요
- The PUT method requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message payload.
- 요청한 URI에 payload(Request Body)에 있는 자원으로 대체(replace)하는 메서드, 대체하는 메서드라는 것은 대상을 저장하기도, 변경한다는 것을 의미한다.
- 요청한 URI 아래에 자원이 존재하지 않는 경우: POST와 같이 새로운 자원으로 저장하고 클라이언트에게 Http Status Code를 201(Created) 응답을 보내주면 된다.
- 요청한 URI 아래에 자원이 존재하는 경우: payload에 담긴 정보를 이용해서 새로운 자원을 만들어 기존에 존재하던 자원을 대체한다. 그리고 해당 요청이 잘 적용되었다는 것을 클라이언트에게 200(ok) 혹은 204(no content)를 이용해서 알려준다.
- PUT 메서드를 사용하는 클라이언트는 해당 자원의 상태를 모두 알고 있다고 가정되어야 한다.
- PUT 메서드는 요청 경로에 자원이 존재하는 경우 해당 자원을 payload 정보와 교체하는 메서드이다. 즉, PUT 메서드를 사용할 때 전송하는 payload만으로 자원의 전체 상태를 나타낼 수 있어야 한다. 새로운 자원을 생성해야 하는 경우 완전한 상태의 자원을 저장해야 하고 새로운 자원으로 대체하는 경우 대체하는 자원이 완전한 상태를 가지고 있어야 하기 때문이다.
만약 PUT의 정의대로 전달 받은 payload가 기존 정보를 대체하도록 구현한 경우 payload 정보가 불완전한 상태로 전송된다면 일부 entity의 field값들은 null로 변경될 수 있다.

### PATCH
- 자원의 부분 교체, 자원교체시 일부 필드 필요
- This specification defines the new HTTP/1.1 [RFC2616] method, PATCH, which is used to apply partial modifications to a resource.
- PATCH는 부분 수정을 위한 데이터만 요청의 payload로 보내기 때문에 아래와 같이 body를 받는 DTO를 별도로 만들어 주어야 하고, 이 부분 데이터를 받는 DTO로는 새로운 엔티티를 생성할 수 없고 오직 부분 수정을 위한 데이터로써의 준비를 마치게 된다.

### PATCH는 멱등성을 보장하지 않는다.
- 멱등성: 어떤 대상에 같은 연산을 여러번 적용해도 결과가 달라지지 않는 성질이다.
- PATCH 메소드는 구현 방법에 따라서 PUT 메소드처럼 멱등성이 보장될 수도 있고, 혹은 보장되지 않을 수도 있다.
- PATCH 메소드는 PUT 메소드처럼 리소스를 대체하는 행위가 아니기 때문에 요청을 어떤 방식으로 사용하는지에 대한 제한이 없기 때문이다.
- RFC 스펙 상의 PATCH 메소드는 단지 리소스의 일부를 수정한다는 의미만을 가질 뿐이다.
```
// 멱등하지 않는 예시: 한번 호출할 때 마다 나이를 10 더하는 식으로 변경하고 싶다고 가정
다음과 같이 특정 부분을 추가로 더하거나 하는 식으로 설계해도 된다. 이 경우 서버에서 operation add가 어떤 의미인지 알 수 있어야 함
{ "operation": "add", "age": 10"}
2번 호출하면 +10 +10이 되어서 멱등이 아님
````

출처 
https://ko.wikipedia.org/wiki/REST
http://meetup.toast.com/posts/92
https://tecoble.techcourse.co.kr/post/2020-08-17-put-vs-patch/
