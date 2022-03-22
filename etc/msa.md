# MSA

### 작고 한 가지 일을 잘하는 데 주력
- 자율적으로 협업하는 서비스
- 단일 책임 원칙: 같은 이유로 변경되는 것들은 한데 모으고, 서로 다른 이유로 변경되는 것들을 분리하라
- 독립적인 서비스에 대해 동일한 접근 방식
- 충분히 작아서 더 이상 작아질 수 없는 크기
- 서비스 간 분산도를 높이고 서비스 간에 밀접하게 연결되어 생기는 문제점을 줄이기 위해서 서비스 사이의 모든 통신은 네트워크 호출을 통해 이루어진다.
- 기술 이기종성: 각 서비스가 다른 기술을 사용하도록 결정

### Event Driven Architecture
- Event: 시스템 내부 외부에서 유발된 시스템 상태 중요 변화 및 의미있는 사건
- 분산 시스템에서 비동기 통신 방식으로 이벤트 발행/구독 아키텍처
- 동기 통신: API 통한 peer to peer / peer가 active여야 유실 없이 통신
  - 응답 서비스에서 오랜 시간을 소비하면 그만큼 응답이 늦어지기에 Blocking 모델
  - 요청 체인이 길어지면 장애 전파(Fault Spread)의 위험성
- 비동기 통신: Event Channel(Message Broker, Kafka) 통한 pub/sub
  - 비동기 통신에 Broadcasting, 장애 격리(Fault Isolation) 

### EDM Event Driven Architecture 적용 MicroService
- MSA가 적용된 시스템에서 이벤트 발생시 해당 이벤트 로그를 보관하고 이를 기반으로 동작하며, 비동기 통신을 통해 시스템 내 통합(integration)을 수행하는 Architecture 
- 상태가 변경되면 이벤트를 발생 > 발행하고 이를 관심있는 서비스가 수신 후 기능을 수행 > 비즈니스 흐름에 따라 각 서비스의 기능 수행
- 비동기 통신 사용, 각 MicroService 느슨한 결합도 유지
- EDM 발생 이벤트 > 이벤트 스토어 저장
  - 예) 이벤트를 적절한 소비자에게 전달하도록 보장하는 대기열(Queue)에 게시
  - 예) 이벤트를 게시하고 모든 이해 당사자에게 액세스를 허용하는 "발행-구독"(Pub-Sub) 메시지 모델에 게시
  - 생산자는 이벤트를 발행하고 소비자는 해당 이벤트를 수신하여 그에 따라 반응
- Transaction Management
  - 롤백이 필요한 경우 Failed 이벤트를 발생 -> 이전 스텝을 수행한 서비스에서 구독하여 보관되어 있던 이벤트 로그 기반으로 롤백 수행
  - all commit or rollback → eventually consistency
- Broker Dependency
  - 시스템 간 의존도는 낮아지지만 Broker 에 대한 의존성이 발생
````
Event Generator (Publisher, Producer, Creater)
- 표준화된 형식의 이벤트를 생성(발행) 생성된 이벤트는 Event Channel로 전송

Event Channel (Bus)
- Event Generator에서 Event Processing Engine으로 수집된 데이터를 전파하는 메커니즘
- 이벤트 발송하는 역할

Event Processing Engine (Consumer, Processor)
- 수신한 이벤트를 식별
````

### 참고
- 마이크로서비스 아키텍처 구축
- https://www.samsungsds.com/kr/insights/msa_architecture_edm.html. 
- https://medium.com/dtevangelist/event-driven-microservice-%EB%9E%80-54b4eaf7cc4a