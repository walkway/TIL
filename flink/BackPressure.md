# Back Pressure

- 작업에 대한 역압 경고 (예: High)가 표시되면 다운스트림 작업자가 소비할 수 있는 것보다 더 빠르게 데이터를 생성하고 있음을 의미
- 작업 흐름 다운스트림(예: 소스에서 싱크로)의 레코드와 역압은 반대 방향인 스트림 위로 전파
- 작업(하위 작업) 메트릭 그룹
 - backPressureTimeMsPerSecond: 하위 작업이 압박을 받는 데 소요한 시간
 - idleTimeMsPerSecond: 하위 작업이 처리할 무언가를 기다리는 데 소요한 시간
 - busyTimeMsPerSecond: 하위 작업이 실제 작업을 수행하느라 바빴던 시간
 - 세 가지 메트릭을 합하면 대략 1000ms.

## Checkpointing under backpressure
- Flink 작업이 과도한 backpressure 하에 실행될 때 체크포인트의 end-to-end time에서 지배적인 요소는 모든 operators/subtasks에 체크포인트 barriers을 전파하는 시간이 될 수 있다. 
- 해결 방안
  - Flink 작업을 최적화하거나 Flink 또는 JVM 구성을 조정하거나 확장하여 backpressure 소스를 제거
  - Flink 작업에서 버퍼링된  in-flight data 양 줄이기
  - 정렬되지 않은 체크포인트 활성화

### Buffer debloating
- Flink 1.14에는 Flink operators/subtasks 간에 버퍼링된 in-flight data을 자동으로 제어하는 새로운 도구가 도입되었다.
- taskmanager.network.memory.buffer-debloat.enabled 속성을 true 설정하여 활성화할 수 있다.
- 정렬된 체크포인트와 정렬되지 않은 체크포인트 모두에서 작동하며 두 경우 모두 체크포인트 시간을 개선할 수 있다. 효과는 정렬된 체크포인트에서 가장 잘 보인다.
- 정렬되지 않은 체크포인트와 함께 버퍼 팽창을 사용할 때 추가 이점은 더 작은 체크포인트 크기와 더 빠른 복구 시간이다.

### Unaligned checkpoints
- Flink 1.11부터 체크포인트를 정렬 해제할 수 있다. 정렬되지 않은 체크포인트 는 체크포인트 상태의 일부로 진행 중인 데이터(즉, 버퍼에 저장된 데이터)를 포함하므로 체크포인트 barriers가 이러한 버퍼를 추월할 수 있다.
- backpressure 인해 체크포인트 기간이 매우 긴 경우 정렬되지 않은 체크포인트를 사용해야 한다. 
````
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

// enables the unaligned checkpoints
env.getCheckpointConfig().enableUnalignedCheckpoints();
````

https://flink.apache.org/2021/07/07/backpressure.html  
https://www.ververica.com/blog/how-flink-handles-backpressure  
https://flink.apache.org/2021/01/18/rocksdb.html  
https://nightlies.apache.org/flink/flink-docs-master/docs/ops/state/checkpointing_under_backpressure/
