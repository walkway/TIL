# siddhi

### quick start
- 화물 상자를 선박에 적재하는 시나리오
- 이벤트: 선적시 화물 상자의 무게를 측정하는 것

1. 이름 부여
````
@App : name ( "HelloWorldApp")
````

2. 입력 스트림 정의
- 스트림은 이벤트의 이름, 스키마 필요
````
define stream CargoStream (weight int);
````

3. 출력 스트림 정의
- Sink: Siddhi에서 외부 시스템에 스트림을 게시하는 방법
````
@sink(type='log', prefix='LOGGER')
define stream OutputStream(weight int, totalWeight long);
````

4. Siddhi query
- 쿼리 이름: "HelloWorldQuery"
- 처리 스트림: "CargoStream"
- 필요 데이터: "weight", "totalWeight"
- 출력 계산: 합
- 출력 스트림: "OutputStream"
````
@info(name='HelloWorldQuery')
from CargoStream
select weight, sum(weight) as totalWeight
insert into OutputStream;
````

5. 시뮬레이션
- start, send
````
Siddhi App Name: HelloWorldApp
Stream Name: CargoStream
Timestamp: (Leave it blank)
weight: 2 (or some integer)
````
- 결과
````
HelloWorldApp.siddhi - Started Successfully!
[2018-08-20_16-57-31_778] INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - LOGGER : Event{timestamp=1534751851772, data=[2, 2], isExpired=false} 
[2018-08-20_17-27-55_598] INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - LOGGER : Event{timestamp=1534753675596, data=[2, 4], isExpired=false} 
````

````
@App:name("HelloWorldApp")

define stream CargoStream (weight int);

@sink(type='log' , prefix='LOGGER')
define stream OutputStream(weight int, totalWeight long);

@info(name='HelloWorldQuery')
from CargoStream
select weight, sum(weight) as totalWeight
insert into OutputStream;
````

6. temporal event processing
- window processing: 일정 기간 동안 메모리에 이벤트를 저장 -> 평균, 최대 값 등 계산하는 작업 수행
- 최근 적재된 상자 평균 중량, 마지막 3개 상자의 평균 값
- 메모리 내 3개의 이벤트만 유지
````
@App:name("HelloWorldApp")

define stream CargoStream (weight int);

@sink(type='log' , prefix='LOGGER')
define stream OutputStream(weight int, totalWeight long, averageWeight double);

@info(name='HelloWorldQuery') 
from CargoStream#window.length(3)
select weight, sum(weight) as totalWeight, avg(weight) as averageWeight
insert into OutputStream;
````

##### 출처: https://wso2.github.io/siddhi/documentation/siddhi-quckstart-4.0/