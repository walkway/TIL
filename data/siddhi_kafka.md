# siddhi io kafka

### Prerequisites
- Download the Kafka broker from here: https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz
- Convert and copy the Kafka client jars from the {KafkaHome}/libs directory to the {WSO2SPHome}/libs directory as follows.
- Create a directory named {Source} in a preferred location in your machine and copy the following JARs to it from the {KafkaHome}/libs directory.
````
kafka_2.11-0.10.2.1.jar
kafka-clients-0.10.2.1.jar
metrics-core-2.2.0.jar
scala-library-2.11.8.jar
scala-parser-combinators_2.11-1.0.4.jar
zkclient-0.10.jar
zookeeper-3.4.9.jar
````
- Create another directory named {Destination} in a preferred location in your machine.
- To convert all the Kafka jars you copied into the {Source} directory, issue the following command,
````
{WSO2SPHome}/bin/jartobundle.sh <{Source} Directory Path> <{Destination} Directory Path>
````
- Add the OSGI converted kafka libs from {Destination} directory to {WSO2SPHome}/lib
- Add the original Kafka libs from {Source} to {WSO2SPHome}/samples/sample-clients/lib
- Navigate to {KafkaHome} and start zookeeper node using "sh bin/zookeeper-server-start.sh config/zookeeper.properties" command
- Navigate to {KafkaHome} and start Kafka server node using "sh bin/kafka-server-start.sh config/server.properties" command
- Start the server: sh editor.sh

### Sample
````
@App:name('KafkaTest') 

@source(type='kafka',
        topic.list='test',
        partition.no.list='0',
        threading.option='single.thread',
        group.id="test-consumer-group",
        bootstrap.servers='localhost:9092',
        @map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='log' , prefix='LOGGER')
define stream  LowProductionAlertStream (name string, amount double);

--Send events in a length window of 5 from kafka_topic to kafka_result_topic
@info(name='query1')
from SweetProductionStream#window.length(5)
select *
insert into LowProductionAlertStream;
````

````
./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
````
- input
````
{"name" : "hello","amount" : 3.0}
````
- output
````
 INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - LOGGER : Event{timestamp=1534902026164, data=[hello, 3.0], isExpired=false}
````

##### 참고 https://wso2-extensions.github.io/siddhi-io-kafka/
