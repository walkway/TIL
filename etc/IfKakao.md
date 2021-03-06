# If Kakao

## 카프카, 산전수전 노하우

### 현황
- 용어
  - 프로듀서: 카프카로 메시지를 보내는 역할
  - 카프카: 프로듀서가 보낸 메시지 저장
  - 컨슈머: 카프카에 저장된 메시지를 가져옴
  - 주키퍼: 메타 데이터 관리, 클러스터 노드 관리
- 카프카 클러스터 처음 구성하는 경우: 카프카 3대, 주키퍼 3대 최소 총 6대 서버 필요
- 사용량이 늘어나서 서버 증설이 필요한 경우: 주키퍼는 그대로 두고, 카프카만 서버를 추가하여 스케일 아웃
- 카카오는 전사 공용 카프카를 운영하고 있음. 처음 [2개 클러스터, 10대 서버] -> 현재 [7개 클러스터 130대 서버] 운영 중
  - 메인으로 사용하는 IDC 2곳에서 주키퍼 세트를 두고, 카프카 클러스터를 연결해서 총 7개 클러스터를 운영
- 카카오에서 하루에 카프카 메세지 건수: 약 2600억개 정도 -> 초당 300만건 메세지 처리
- 카프카로 들어오는 데이터 크기: 하루에 in 240 테라바이트, out 370 테라바이트
- 가동률 99.99퍼센트 (1시간/2년)

### 트러블 슈팅
1. shrinking ISR (ISR 축소되는 것)
- ISR
  - In Sync Replicas
  - 리플리케이션 구성원: 리더, 팔로워
  - 리더: 읽고 쓰기
  - 팔로워: 리더와 주기적 동기화
  - ISR 구성원만 리더의 자격을 가짐
- 카프카 로그 레벨: info, warning, error
- 문제 시(warning, error) 알람 받도록 하고 있었는데, 알람이 오지 않았음.
  - shrinking ISR: info 로그에서 확인할 수 있었음.
  - info 로그 확인해보는 것도 중요
- 카프카 버전 업그레이드 방식 2개
  - 다운 타임 o: 모든 브로커를 내리고 최신버전으로 업그레이드, 브로커 실행
  - 다운 타임 x: 클러스트 내, 브로커 한대씩 내렸다가 버전 업그레이드, 전체 클러스터가 다운되지 않는 상황에서 진행
- 카카오에서는 다운 타운없이 업그레이드 진행

2. 전원 이슈 Rack 
- IDC에 있는 서버, Rack(선반같은..?)에 원하는 서버를 두고 사용
- 브로커 1 2 3을 Rack1에만 두었을 때, 문제가 생기면 클러스터 전체가 다운될 수 있음
- 각 Rack마다 브로커 서버를 분산해서 두는 것이 효율적
- 한번에 여러개의 Rack의 전원이 나갔던 상황 -> 모든 브로커가 다운되었음
- ex)
  - Rack1-브로커 1, Rack2-브로커 2, Rack3-브로커3이 차례로 장애가 났음.
  - Rack3이 제일 먼저 복구, Rack1, Rack2도 복구
  - 장애 전, 마지막 리더(브로커3)가 새로운 리더(브로커3)가 됨. 브로커1, 브로커2가 리더로부터 메시지 동기화 -> 메시지 손실 없음
  - 가장 먼저 다운된 브로커1이 복구되어 리더가 됨, 동기화 -> 메시지 손실 생길 수 있음
- 장애 복구 우선 순위
  - 데이터의 정합성: 마지막 리더를 기다리는 옵션 -> 메시지 손실 없음
  - 서비스의 영속성: 마지막 리더가 복구되지 않으면 -> 장애시간이 길어짐
- 카카오에서는 최대한 장애 타임을 줄이고 서비스에 투입
  - 서비스의 영속성 우선
  - 일부 메시지 손실 감수

### 프로듀서
- 파티션의 리더, 메시지 전달
- 배치 처리 가능, 배치 크기, 지연 시간 조정 가능
- 프로듀서 ACKS: 0, 1, ALL
  - 0: 보내고 확인하지 않음, 메시지 손실 위험 있음
  - 1: 파티션의 리더가 받았는지 확인, 가장 많이 사용하는 옵션
  - all: 팔로우도 받았는지 확인
- 프로듀서 key
  - 하나의 파티션으로만 메시지를 보낼 수 있음. key 값 필수 아님
  - key 넣지 않는 경우 none, 라운드 로빈 형식으로 균등하게 분배

### 컨슈머
- 파티션 리더에게 fetch 요청
- 위치(오프셋)를 보고 메시지를 가져옴 (메시지 내용을 보고 가져오는 것이 아님)
- 파티션이 하나인 경우 순서 보장
- 파티션 n개인 경우, 각 파티션의 오프셋 순서만 보장 (메시지 순서대로 하고싶으면, 메시지 특정 필드에 타임스탬프 -> 정렬)
- 컨슈머 그룹: 하나의 토픽을 여러 컨슈머들 구독
- LAG: 컨슈머가 얼마나 밀리지 않고, 메시지를 가져가고 있는지 확인하는 지표
  - Burrow: Lag checking open source https://github.com/linkedin/Burrow

### 운영
- 미사용 토픽: 삭제에 대한 요청이 거의 안옴
  - JMX로 토픽 상태값을 수집 저장 후 조건에 일치하는 토픽 삭제
- 중요한 토픽은 삭제되기 3일 전부터 추가로 알람

### QnA
- 클러스터 나누는 이유, 기준
  - 버전 업그레이드, config 수정할 때, 번거로움
  - 1개 클러스터에 20~30개 정도..?
- 다운 타임
  - 많은 카카오 서비스에서 카프카 사용 중, 다운 타임을 가질 수 없는 상황
  - 테스트 환경에서 테스트 후, 버전 업그레이드
- best practice
  - 진행 중~
- 한 클러스터 내에서 적정한 토픽 수
  - 카카오: 한 클러스터에 500개 미만
  - 크게 자원을 소비하지않아도 브로커는 계속 토픽을 바라보고 있음. 불필요한 토픽은 최대한 삭제 권장


## 카카오톡의 서버사이드 코틀린
- 카카오톡 서버 구조 개선 프로젝트 개발 언어로 코들린 도입
- 선택 배경
  - 적정한 성능/안정성, 개발/운영의 편의성
  - 다양한 목적의 서버 환경에 검증된 자바의 생태계 이용
  - 생산성 좋은 새로운 언어
  - 적정 수준 러닝 커브

### 개발환경: 개발 환경 쓸만한가
- 안정화된 언어, 개발환경
- 자바 to 코틀린 코드 변환

### 생태계: 자바와 어느 정도 호환되나
- 자바 호환 100퍼센트
- 클래스, 인터페이스, 함수 호환
- 어노테이션 적용
- 기본 데이터 타입
- 프레임웍 러닝 커브 없음

### 문법: 언어로 생산성과 안전함이 향상된다
- 중복 최소화
- 안전한 코드
- nullable 타입을 문법에서 구분
- primitive 타입, boxed 타입 구분 없음: Int, Long, Float ... 클래스 이용
- 특수 타입: Any, Nothing, Unit
- nullable 타입: not-null type, nullable type
- 불변컬렉션: immutable list, immutable map
- 불변 변수 val를 권장 (final 변수), 상속 제한이 디폴트(final 클래스, 함수)
- 명확한 코드
````
StringUtils.isEmpty(str) -> str.isNullOrEmpty()
````
### 비교
- 코드 라인 수: 자바8 > 코틀린 (2/3가 줄었음)
- 바이트코드 사이즈: 자바 8 < 코틀린 (2배 늘었음)
- 컴파일 시간: 자바8 < 코틀린 (6배 늘었음)
- 성능 비슷

### 마이그레이션
- 자바 코드/클래스 연동시 타입 주의
- Lombok 적용 자바 코드와 같은 모듈에서 사용하기 힘듬
- Spring에서 특정 클래스, open 해줘야 함
- Immutable 데이터로 리팩토링하는 것은 생각보다 큰 일


## 다음웹툰의 UX(Animation, Transition, Custom View)
- 웹툰 2.0
- 디자이너와 협업해서 만듬

### interaciton keyword, 동일한 컨셉
- creative: 다르게
- natural: 갑작스러운 변화는 아니고, 자연스럽게
- focusing: 시선이 여러곳으로 분산되지 않게
- meaningful: 디자인, 개발에 의미가 있어야함

### work process
- 1. design prototyping
- 2. consultation
- 3. development prototyping
- 4. review
- (1 ~ 4 반복: 디자이너와 개발자가 만족할 때까지)
- 5. output

### home interacion : splash
- 메인화면 구성에 필요한 서버 통신 시간을 버는 의미
- 디자인 컨셉: webtoon frame - vertical, contents, container
- 사각형들이 합쳐지면서..
- issue
 - 사각형의 움직임 3개, 가속도 2개, 컨트롤이 힘듬
 - 발상의 전환: 빨간 사각형 3개 -> 빨간 사각형 1개, 흰색 사각형 2개

### symbolic icon
- request: 탭 이동할 때, icon이 움직이게 (동영상 or gif)
- issue: 빠르게 탭 이동시 생기는 문제들 메모리 이슈 등
- solution: 아이콘을 패스로 구현 -> 디자인 컨셉 수정
- 각 좌표 거리가 멀어서 움직임이 부자연스러움 -> 중간에 마름모 모양을 넣음

### list animation
- 계단 형태 animation
- custom animation
- 디폴드 코드 복사해서 callback 추가

### title page
- activityTransition
- SharedElement
- issue
 - SharedElement로 사용하는 것이 느릴 때: 애니메이션 생략
 - getLocationInwindow값이 StatusBar Height만큼 어긋남?
 - changeBoundas가 오동작해서 이미지 Scale이 어긋남 -> 이미 커진상태로 이미지가 올라감..?
- solution
 - custom
- add
 - curve animation
 - ripple effect: 물방울이 터지듯

### 정리
- creative: splash
- natural: symbolic icon
- focusing: ripple, list
- meaningful: all

### QnA
-  중간부터 코틀린으로 바뀐 이유
  - 자바 -> 작년 11월부터 변경, 80퍼센트 변경 함
- 기간
  - 5개월, 안드로이드 2명 디자이너 2명 기획자 2명
- qa단계
  - 따로 전문적인 팀이 있음. 릴리즈 1주일 전에
- 프로토타이핑
  - 디자이너: 에프터 이펙트, 개발자: 안드로이드 스튜디오