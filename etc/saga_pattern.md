# saga pattern

- 분산 애플리케이션의 일관성을 유지하고 여러 마이크로서비스 간의 트랜잭션을 조정하여 데이터 일관성을 유지하는 데 도움이 되는 장애 관리 패턴
- 마이크로서비스들끼리 이벤트를 주고 받아 특정 마이크로서비스에서의 작업이 실패하면 이전까지의 작업이 완료된 마이크서비스들에게 보상 (complemetary) 이벤트를 실행하여 분산 환경에서 원자성(atomicity)을 보장한다.
- 어플리케이션은 긴밀한 연결 없이 여러 마이크로서비스 간에 데이터 일관성을 유지해야 한다.
- 오래 지속되는 트랜잭션이 있으며 한 마이크로서비스가 오랫동안 실행될 경우 다른 마이크로서비스가 차단되는 것을 원하지 않는다.
- 시퀀스에서 작업이 실패할 경우 롤백할 수 있어야 한다.

## Choreography
- 서비스 간 이벤트를 주고 받는 Event Pub/Sub 
- 각 서비스가 메시지 브로커에 메시지를 전송 / 구독하는 서비스는 이벤트가 발생하면 트랜잭션을 수행하고 다시 이벤트 발생
- 트랜잭션이 실패하면 취소된 어플리케이션에서 보상 이벤트를 발행하여 롤백을 시도한다.
- 메인이 되는 서비스에서 관련 서비스의 로컬 트랜잭션을 호출한다.
- 특정 서비스에서 로컬 트랜잭션 오류가 발생하면 해당 서비스에서 자신을 호출한 서비스의 Rollback service 호출한다.

## Orchestration

https://docs.aws.amazon.com/ko_kr/prescriptive-guidance/latest/modernization-data-persistence/saga-pattern.html  
https://vinsguru.medium.com/choreography-saga-pattern-with-spring-boot-microservice-design-patterns-fb35a1802bee  