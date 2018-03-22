# Kafka
- 분산 메시징 시스템
- 대용량의 실시간 로그처리에 특화
- 파일 시스템 이용: 메모리를 저장하는 구조가 아니기 때문에 데이터 자체의 휘발성이 없다.
- 발행-구독(publish-subscribe) 모델을 기반
- producer, consumer, broker
- 하나의 파티션에 대해서 데이터의 순서를 보장한다.
- 토픽에 대해 모든 데이터 순서를 보장받고 싶다면, 토픽을 생성할 때, 파티션의 수를 1로 한다.
- 컨슈머는 파티션 첫번째 데이터를 가져오고, 파티션의 순서대로 가져오지 않는다.

### ZooKeeper
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

출처
http://epicdevs.com/17

http://www.popit.kr/kafka-%EC%9A%B4%EC%98%81%EC%9E%90%EA%B0%80-%EB%A7%90%ED%95%98%EB%8A%94-%EC%B2%98%EC%9D%8C-%EC%A0%91%ED%95%98%EB%8A%94-kafka/

http://paulsmooth.tistory.com/69

https://kafka.apache.org/quickstart