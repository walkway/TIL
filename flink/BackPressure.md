# Back Pressure

- 작업에 대한 역압 경고 (예: High)가 표시되면 다운스트림 작업자가 소비할 수 있는 것보다 더 빠르게 데이터를 생성하고 있음을 의미
- 작업 흐름 다운스트림(예: 소스에서 싱크로)의 레코드와 역압은 반대 방향인 스트림 위로 전파
- 작업(하위 작업) 메트릭 그룹
 - backPressureTimeMsPerSecond: 하위 작업이 압박을 받는 데 소요한 시간
 - idleTimeMsPerSecond: 하위 작업이 처리할 무언가를 기다리는 데 소요한 시간
 - busyTimeMsPerSecond: 하위 작업이 실제 작업을 수행하느라 바빴던 시간
 - 세 가지 메트릭을 합하면 대략 1000ms.

https://flink.apache.org/2021/07/07/backpressure.html  
https://www.ververica.com/blog/how-flink-handles-backpressure
https://flink.apache.org/2021/01/18/rocksdb.html
