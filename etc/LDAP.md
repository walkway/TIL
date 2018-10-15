# LDAP

- LDAP (Lightweight Directory Access Protocol)
- DAP(Directory Access Protocol): TCP/IP 위에서 디렉토리 서비스를 조회하고 수정하는 응용 프로토콜
- X.500 디렉토리 서비스를 위한 DAP(Directory Access Protocol)의 일종
- 회사내의 인트라넷(인터넷)에 접속해서, 필요한 정보가 서버, 어떤 디렉토리에 위치해 있는지를 알수 있게 만든 규칙

### 옵션
- dn: distinguished name 여러가지 엔트리들이 모여 특정한 사용자(혹은 물체)를 구분할 수 있는 고유의 이름
- o: organization
- cn: common name 일반적인 이름
- c: country
- rdn: relative distinguished name
- ou: organizational unit 그룹
- sn: surname 이름 성
- dc: domain component 도메인 요소 naver.com
- ObjectClass: 동일한 속성을 갖고 있는 일종의 그룹. 모든 엔트리는 ObjectClass에 속하고, ObjectClass의 정의대로 attribute를 가지게 된다.

http://egloos.zum.com/js7309/v/11068093
https://www.youtube.com/watch?v=fuG5YziN_xs