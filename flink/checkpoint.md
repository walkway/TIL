# Checkpoint

- Flink의 모든 함수와 연산자는 상태를 저장할 수 있다. 상태 저장 기능은 개별 요소/이벤트 처리 전반에 걸쳐 데이터를 저장하므로 상태를 모든 유형의 보다 정교한 작업을 위한 중요한 구성 요소로 만든다.
상태 내결함성을 만들기 위해 Flink는 상태를 체크포인트 해야한다. 체크포인트를 통해 Flink는 스트림의 상태와 위치를 복구하여 애플리케이션에 오류 없는 실행과 동일한 의미를 부여할 수 있다.
- Flink의 검사점 메커니즘은 스트림 및 상태에 대한 내구성 있는 저장소와 상호 작용한다. 
- 일정 시간 동안 레코드를 재생할 수 있는 영구 (또는 지속) 데이터 원본이다. 영구 메시지 대기열(예: Apache Kafka, RabbitMQ, Amazon Kinesis, Google PubSub) 또는 파일 시스템(예: HDFS, S3, GFS, NFS, Ceph 등)이 있다.
- 상태용 영구 스토리지, 일반적으로 분산 파일 시스템(예: HDFS, S3, GFS, NFS, Ceph 등)

## Stateful
- 데이터 흐름이 많은 작업은 단순히 한 번의 하나 개별 이벤트를 보는 반면 일부 작업은 여러 이벤트의 정보를 기억한다.
- 상태 저장 작업 예시
  - application 특정 이벤트 패턴 검색할 때, 상태는 지금까지 발생한 이벤트 시퀀스를 저장
  - 분/시/일 이벤트를 집계할 때 상탠ㄴ 보류 중인 집계를 보유
  - Checkpoint, Savepoint를 사용하여 내결함성을 만들기 위해 상태를 인식

### State 지속성
- Flink는 스트림 재생, 체크포인트 조합으로 내결함성을 구현
- 체크포인트는 각 연산자에 대한 해당 상태와 입력 스트림의 특정 지점 표시
- 메커니즘 보장을 위해서는 데이터 스트림 소스가 스트림을 정의된 최근 지점으로 되감을 수 있어야 한다.

### barriers
- 스트림 장벽은 데이터 스트림에 주입되고 데이터 스트림의 일부로 레코드와 함께 흐름
- 장벽은 기록을 추월하지 않고, 일직선으로 흐름
- 장벽은 데이터 스트림의 레코드를 현재 스냅샷으로 이동하는 레코드 세트와 다음 스냅샷으로 이동하는 레코드로 분리
- 각 장벽은 그 앞에 푸시한 레코드가 있는 스냅샷의 id를 전달
- 다른 스냅샷의 여러 장벽이 동시에 스트림에 있을 수 있음
- 둘이 상의 입력 스트림을 수신하는 operator는 스냅샷 장벽에 입력 스트림을 정렬해야 한다.
- operator는 들어오는 스트림에서 스냅샷 장벽 n을 받자마자 다른 입력에서도 장벽 n을 받을 때 까지 해당 스트림에서 더 이상 레코드를 처리할 수 없다.

## checkpoint vs savepoint
### checkpoint
- 사용자가 애플리케이션을 종료하면 체크포인트는 자동으로 삭제(체크포인트가 유지되도록 명시적으로 구성된 경우 제외).
- 체크포인트는 상태 백엔드별(네이티브) 데이터 형식으로 저장

### savepoint
- 저장점은 전적으로 사용자가 생성, 소유 및 삭제. 즉, Flink는 작업 종료 후나 복원 후에도 저장점을 삭제하지 않음.
- 저장점은 상태 백엔드 독립(표준) 형식으로 저장 (참고: Flink 1.15 이후로 저장점은 생성 및 복원이 더 빠르지만 몇 가지 제한이 있는 백엔드별 기본 형식으로도 저장할 수 있음)

````
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

// start a checkpoint every 1000 ms
env.enableCheckpointing(1000);

// advanced options:

// set mode to exactly-once (this is the default)
env.getCheckpointConfig().setCheckpointingMode(CheckpointingMode.EXACTLY_ONCE);

// make sure 500 ms of progress happen between checkpoints
env.getCheckpointConfig().setMinPauseBetweenCheckpoints(500);

// checkpoints have to complete within one minute, or are discarded
env.getCheckpointConfig().setCheckpointTimeout(60000);

// only two consecutive checkpoint failures are tolerated
env.getCheckpointConfig().setTolerableCheckpointFailureNumber(2);

// allow only one checkpoint to be in progress at the same time
env.getCheckpointConfig().setMaxConcurrentCheckpoints(1);

// enable externalized checkpoints which are retained
// after job cancellation
env.getCheckpointConfig().setExternalizedCheckpointCleanup(
    ExternalizedCheckpointCleanup.RETAIN_ON_CANCELLATION);

// enables the unaligned checkpoints
env.getCheckpointConfig().enableUnalignedCheckpoints();

// sets the checkpoint storage where checkpoint snapshots will be written
env.getCheckpointConfig().setCheckpointStorage("hdfs:///my/checkpoint/dir");

// enable checkpointing with finished tasks
Configuration config = new Configuration();
config.set(ExecutionCheckpointingOptions.ENABLE_CHECKPOINTS_AFTER_TASKS_FINISH, true);
env.configure(config);
````

https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/fault-tolerance/checkpointing/
