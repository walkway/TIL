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
 - 각각의 그룹내의 서버들끼리는 서로의 정보를 공유하고 있어, 만약 하나의 서버가 Down되더라도 다른 서버가 그 서버의 역할을 할 수 있음.
- 컨슈머 인스턴스: 하나의 프로세스 or 서버
- offset: 파티션안에서 데이터 위치
  - 컨슈머 그룹들을 구분하고, 컨슈머 그룹들은 자신의 그룹에 대한 offset 관리한다.
- 컨슈머 그룹, 파티션 수 관계
  -  하나의 파티션에 대해 컨슈머 그룹내 하나의 컨슈머 인스턴스만 접근 가능
  -  파티션에 대해 한명의 reader만 허용하여 데이터를 순서대로 읽어갈 수 있게 하기 위함
  - 파티션 수보다 컨슈머 그룹의 인스턴스 수가 많을 수 없음
  - 토픽의 파티션 수와 컨슈머 인스턴스 수는 동일하게 맞추어 주거나 절반정도 수준으로 구성
  - 토픽의 파티션 수는 토픽이 생성된 이후에 언제든지 늘릴 수 있지만, 절대로 줄일 수는 없음
- producer acks: 프로듀서가 메시지를 보내고 그 메시지를 카프카가 잘 받았는지 확인을 할 것인지 또는 확인을 하지 않을 것인지를 결정하는 옵션
 - producer에서 acks=0 이라고 설정하면, 프로듀서는 메시지를 보내고 leader로부터 보낸 메시지에 대해 잘 받았는지 확인을 기다리지 않는다.

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

##### 출처
http://epicdevs.com/17
http://www.popit.kr/kafka-%EC%9A%B4%EC%98%81%EC%9E%90%EA%B0%80-%EB%A7%90%ED%95%98%EB%8A%94-%EC%B2%98%EC%9D%8C-%EC%A0%91%ED%95%98%EB%8A%94-kafka/
http://paulsmooth.tistory.com/69
https://kafka.apache.org/quickstart
https://www.popit.kr/author/peter5236
https://docs.confluent.io/platform/current/kafka-rest/quickstart.html