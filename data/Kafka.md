# Kafka
- 분산 메시징 시스템
- 대용량의 실시간 로그처리에 특화
- 파일 시스템 이용: 메모리를 저장하는 구조가 아니기 때문에 데이터 자체의 휘발성이 없다.
- 발행-구독(publish-subscribe) 모델을 기반
- producer, consumer, broker
- 하나의 파티션에 대해서 데이터의 순서를 보장한다.
- 토픽에 대해 모든 데이터 순서를 보장받고 싶다면, 토픽을 생성할 때, 파티션의 수를 1로 한다.
- 컨슈머는 파티션 첫번째 데이터를 가져오고, 파티션의 순서대로 가져오지 않는다.
- 토픽: 데이터베이스 table과 비슷한 개념
  - replica: 복제
  - partition: 토픽을 몇개로 나누는지
- 컨슈머 그룹: 컨슈머 인스턴스를 대표하는 그룹
 - 각각의 그룹내의 서버들끼리는 서로의 정보를 공유하고 있어, 만약 하나의 서버가 Down되더라도 다른 서버가 그 서버의 역할을 할 수 있다.
- 컨슈머 인스턴스: 하나의 프로세스 or 서버
- offset: 파티션안에서 데이터 위치
  - 컨슈머 그룹들을 구분하고, 컨슈머 그룹들은 자신의 그룹에 대한 offset 관리한다.
- 컨슈머 그룹, 파티션 수 관계
  - 하나의 파티션에 대해 컨슈머 그룹내 하나의 컨슈머 인스턴스만 접근 가능
  - 파티션에 대해 한명의 reader만 허용하여 데이터를 순서대로 읽어갈 수 있게 하기 위함
  - 파티션 수보다 컨슈머 그룹의 인스턴스 수가 많을 수 없음
  - 토픽의 파티션 수와 컨슈머 인스턴스 수는 동일하게 맞추어 주거나 절반정도 수준으로 구성
  - 토픽의 파티션 수는 토픽이 생성된 이후에 언제든지 늘릴 수 있지만, 절대로 줄일 수는 없음
- producer acks: 프로듀서가 메시지를 보내고 그 메시지를 카프카가 잘 받았는지 확인을 할 것인지 또는 확인을 하지 않을 것인지를 결정하는 옵션
````
ack=0: Producer가 메시지 전송 후, ack를 기다리지 않고 바로 다음 메시지를 전송한다. 만약 Broker가 다운된다면 이 기간 동안 전송된 메시지는 손실된다.
ack=1: Producer가 메시지 전송 후, partition leader로부터 일정시간 ack를 기다린다. ack 전송 직후 partition leader의 Broker가 follower들이 복사해가기 전에 다운되면 해당 메시지는 손실된다.
ack=all: Producer가 메시지 전송 후, partition의 leader, follower 모두로부터 ack를 기다린다. 손실이 없지만 전송 속도가 느리다.
````

### At-Most-Once
- 최대 한번
- 확인 시간이 초과되거나 오류가 반환될 때 Producer가 재시도하지 않으면, 메시지가 Kafka Topic에 기록되지 않아 Consumer 에게 전달되지 않을 수 있음
- 메시지가 전달되지 않을 수 있음을 허용

### At-Least-Once
- 최소 한번
- 메시지가 최소 1번 이상 전달되는 것 보장한다. 실패나 타임아웃 등이 발생하면 메시지를 다시 전송하고, 동일한 메시지가 중복으로 처리될 수 있다.

### Exactly-Once
- 정확히 한번
- Producer가 메시지 전송을 다시 시도하더라도 메시지가 최종 Consumer에게 정확히 한 번 전달됨
````
하나의 트랜잭션내의 모든 메시지가 모두 Write 되었는지, 되지 않았는지 확인
ID를 할당하고 Header에 고유 정보를 포함하여 식별
broker는 메모리에 Producer ID : Sequence Number 저장하고, 중복 retry가 발생한 경우 체크하여 write를 막음

enable.idempotence: true
Producer 고유한 transactional.id 설정
Consumer isolation.level:read_committed
````

### ZooKeeper
- 카프카 노드 관리
- Kafka의 노드 관리, 토픽의 offset 정보 등 저장한다.
- 과반수 투표방식 -> 홀수로 구성

### Kafka & Flume 
Flume이 빠르게 생성되는 데이터를 실시간으로 수집하게 되면 이를 최종 목적지에 전달하기 전에 중간에서 안정적으로 buffering(queuing) 처리를 해주기 위해 kafka를 사용한다. 만약 flume이 수집한 데이터를 kafka를 거치지 않고 곧바로 목적지(Hbase, ElasticSearch 등)에 전송하게 되는 경우 목적지에 장애가 발생하면 flume의 channel이 event로 가득차게되고 데이터를 수집할 수 없게된다. 실시간으로 생성되는 event의 경우 데이터 유실이 불가피 함으로 곧 바로 장애로 이어진다. 
Flume의 agent를 여러 계층으로 구성함으로써 위와 같은 문제를 완화시킬 수 있으나 결국은 모든 channel이 가득차는 경우 똑같은 장애로 이어진다. 

### Quickstart
````
bin/zookeeper-server-start.sh config/zookeeper.properties
````

````
bin/kafka-server-start.sh config/server.properties
````

````
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
````

````
bin/kafka-topics.sh --list --zookeeper localhost:2181
````

````
bin/kafka-topics.sh --list --zookeeper localhost:2181
````

````
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
````

````
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
````

### REST Proxy Quick Start
Produce and Consume JSON Messages
````
# Produce a message using JSON with the value '{ "foo": "bar" }' to the topic jsontest
curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
      --data '{"records":[{"value":{"foo":"bar"}}]}' "http://localhost:8082/topics/jsontest"

# Expected output from preceding command
  {
   "offsets":[{"partition":0,"offset":0,"error_code":null,"error":null}],"key_schema_id":null,"value_schema_id":null
  }

# Create a consumer for JSON data, starting at the beginning of the topic's
# log and subscribe to a topic. Then consume some data using the base URL in the first response.
# Finally, close the consumer with a DELETE to make it leave the group and clean up
# its resources.
curl -X POST -H "Content-Type: application/vnd.kafka.v2+json" \
      --data '{"name": "my_consumer_instance", "format": "json", "auto.offset.reset": "earliest"}' \
      http://localhost:8082/consumers/my_json_consumer

# Expected output from preceding command
 {
  "instance_id":"my_consumer_instance",
  "base_uri":"http://localhost:8082/consumers/my_json_consumer/instances/my_consumer_instance"
 }

curl -X POST -H "Content-Type: application/vnd.kafka.v2+json" --data '{"topics":["jsontest"]}' \
 http://localhost:8082/consumers/my_json_consumer/instances/my_consumer_instance/subscription
# No content in response

curl -X GET -H "Accept: application/vnd.kafka.json.v2+json" \
      http://localhost:8082/consumers/my_json_consumer/instances/my_consumer_instance/records

# Expected output from preceding command
  [
   {"key":null,"value":{"foo":"bar"},"partition":0,"offset":0,"topic":"jsontest"}
  ]

curl -X DELETE -H "Content-Type: application/vnd.kafka.v2+json" \
      http://localhost:8082/consumers/my_json_consumer/instances/my_consumer_instance
# No content in response
````

### 아키텍처에서 Kafka를 사용하기 위한 모범 사례
- 메시지의 병렬 처리를 활성화하려면 토픽에 여러 파티션을 만듭니다. 이를 통해 여러 소비자가 메시지를 병렬로 처리할 수 있습니다. 각 파티션은 소비자 그룹 내의 한 소비자만 사용할 수 있습니다. 따라서 소비자 그룹에 여러 소비자가 있는 경우 서로 다른 파티션의 메시지를 사용할 수 있습니다. 따라서 메시지 소비를 병렬화하려면 토픽에 여러 파티션을 생성하십시오.
- 각 메시지는 토픽/파티션을 구독하는 모든 소비자 그룹으로 이동하지만 그룹 내에서는 하나의 소비자에게만 전달됩니다. 따라서 consumer groups 토픽을 구독한 모든 사람이 메시지를 받지만 소비자 그룹 내의 한 소비자만 파티션에서 메시지를 받습니다. 따라서 여러 소비자에게 메시지를 브로드캐스트하려면 서로 다르게 consumer groups 할당 하십시오.
- Kafka에서 메시지 크기의 기본 설정은 1MB입니다. 메시지는 Kafka로 전달되기 전에 압축될 수 있습니다. 단일 주제에 더 많은 데이터를 저장하기 위해 여러 서버에 걸쳐 여러 파티션을 만들 수 있습니다.
발행 또는 이용에 필요한 메시지가 직렬화 가능한지 확인하십시오. 날짜 시간과 중첩 구조를 특별히 주의하십시오. 일반적으로 스키마를 사용하는 것이 좋습니다.
- 함수 seek(TopicPartition, long)를 사용하여 새 위치를 지정합니다.
- 메시지 순서가 중요한 응용 프로그램을 설계하고 메시지 순서를 보장하려는 경우 모든 메시지에서 동일한 파티션 ID를 사용합니다. 그 이유는 순서 보장이 파티션 수준에서 적용되기 때문입니다. 따라서 토픽에 둘 이상의 파티션이 있는 경우 동일한 파티션 ID를 갖기 위해 표시해야 하는 메시지를 확인해야 합니다. 항목의 파티션으로 푸시되는 모든 메시지는 파티션 ID가 동일한 경우 올바르게 정렬됩니다.
- 글로벌 순서 지정을 원하면 단일 파티션 토픽 생성합니다.
- 로그를 관리 가능한 상태로 유지하고 정기적으로 디스크 공간을 모니터링하십시오.
- 내구성 있는 시스템을 설계하려면 Kafka 설정에서 높은 replication factor가 설정되어 있는지 확인하십시오. Kafka는 여러 서버에 걸쳐 각 주제의 파티션에 대한 로그를 복제합니다. 서버에 오류가 발생하면 오류가 발생한 경우 메시지를 계속 사용할 수 있으므로 이러한 복제본에 대한 자동 장애 조치가 허용됩니다. 토픽별로 replication factor를 설정할 수 있습니다. 또한 생산자 배치 크기를 1로 설정할 수 있습니다. 이렇게 하면 각 메시지가 디스크에 저장되고 메시지가 배치로 플러시되지 않습니다. 이는 성능에 영향을 미칩니다. 내구성 있고 가용성이 높은 시스템의 경우 높은 주제 복제를 갖는 것이 중요합니다. 일반적으로 안정적인 장애 조치를 위해 최소 3개의 브로커가 권장됩니다.
- 오래된 메시지를 삭제하려면 키에 대한 이전 이벤트가 제거되고 새 이벤트가 주제에 게시될 때 압축된 토픽 사용합니다.
- Kafka를 보호하려면 TLS 클라이언트 인증서를 사용하고 메시지를 암호화하고 사용자 권한을 추가하십시오.
- 또한 Java DSL 또는 Kafka의 SQL과 유사한 스트리밍 언어를 사용하여 Kafka에 저장된 데이터 스트림을 생성하고 처리할 수 있습니다.

### offset
- offset reset
````
--shift-by : Long (+/- 모두 가능)
--to-offset : Long
--by-duration  : PnDTnHnMnS
--to-datetime  : YYYY-MM-DDTHH:mm:SS.sss
--to-current
--to-latest
--to-earliest
````

- 특정 partition 데이터 offset을 n 지정
````
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group {consumer group} --topic {topic}:{partition} --reset-offsets --to-offset {n} --execute
````

- consumer group describe
````
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-group
````

# zookeeper 제거
아파치 카프카는 아파치 주키퍼를 카프카의 메타데이터를 저장하기 위해서 사용한다. 파티션의 위치 그리고 토픽의 설정 정보 같은 데이터는 카프카 밖에 주키퍼 클러스터에 따로 저장되었다. 앞으로 주키퍼를 제거하고, 카프카 자체적으로 메타데이터를 관리 할수 있게 했다.
두개의 시스템을 사용한다는건 많은 복제를 야기했다. 결국, 카프카는 Pub/sub API가 위에 있는 복제된 분산 로그이다. 주키퍼는 filesystem API 위에 있는 복제된 분산 로그이다. 카프카와 주키퍼는 각각 그것만의 네트워크 소통 방법, 보안, 모니터링, 설정값 들이 있다. 두개의 시스템을 가지고 있는건 운영의 복잡성을 두배 정도 증가 시킨다.잘못된 구성이 보안 침해를 일으킬 위험을 높힌다.
메타데이터를 외부에 저장하면 Kafka의 확장성이 제한된다. 카프카 클러스터가 시작될때 또는 새로운 컨트롤러가 뽑힐때, 컨트롤러는 반드시 주키퍼로부터 클러스터의 모든 상태를 받아서 올려야 한다. 그러므로 메타데이터의 양이 커질수록, 이 로딩 과정은 길어진다. 이러한 상황은 카프카가 저장 할 수 있는 파티션의 개수를 제한한다.
메타데이터를 외부에 저장하면 컨트롤러의 in-memory 상태가 외부 상태로부터 비동기화될 수 있습니다.클러스터에 있는 컨트롤러의 실시간 시점은 ZooKeeper의 시점과 다를 수 있다.
https://www.confluent.io/ko-kr/blog/removing-zookeeper-dependency-in-kafka/

##### 출처
http://epicdevs.com/17
http://www.popit.kr/kafka-%EC%9A%B4%EC%98%81%EC%9E%90%EA%B0%80-%EB%A7%90%ED%95%98%EB%8A%94-%EC%B2%98%EC%9D%8C-%EC%A0%91%ED%95%98%EB%8A%94-kafka/
http://paulsmooth.tistory.com/69
https://kafka.apache.org/quickstart
https://www.popit.kr/author/peter5236
https://docs.confluent.io/platform/current/kafka-rest/quickstart.html

https://medium.com/fintechexplained/12-best-practices-for-using-kafka-in-your-architecture-a9d215e222e3
