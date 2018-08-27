# wso2 stream processor ha

- Minimum High Availability (HA) Deployment
- The recommended HA deployment for WSO2 SP is the two node minimum HA deployment
- active node, passive node
- 두 노드 모두 수신 이벤트를 처리하지만, active 노드만 송신 이벤트로 사용한다.
- active 노드에서 장애가 나면, 중단 지점에서 passive 노드가 active 노드가 된다.
  - 종료 된 active 노드를 다시 시작하면, active 노드와 동기화되어 HA 상태를 유지하고 passive 상태로 
- 2개 노드에 HA가 작동하려면 active 노드와 passive 노드 모두 동일한 (중복) 이벤트를 수신해야한다.
이를 달성하기 위해 아래 나열된 전략 중 하나를 수행 할 수 있다.
  - Kafka와 같은 메시지 브로커를 사용하면 두 노드가 동일한 topic을 구독하여, 중복 이벤트를 수신 할 수 있다.

##### 출처 https://docs.wso2.com/display/SP400/Minimum+High+Availability+%28HA%29+Deployment