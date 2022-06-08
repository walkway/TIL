# Checkpoint

- Flink의 모든 함수와 연산자는 상태를 저장할 수 있다. 상태 저장 기능은 개별 요소/이벤트 처리 전반에 걸쳐 데이터를 저장하므로 상태를 모든 유형의 보다 정교한 작업을 위한 중요한 구성 요소로 만든다.
상태 내결함성을 만들기 위해 Flink는 상태를 체크포인트 해야한다. 체크포인트를 통해 Flink는 스트림의 상태와 위치를 복구하여 애플리케이션에 오류 없는 실행과 동일한 의미를 부여할 수 있다.
- Flink의 검사점 메커니즘은 스트림 및 상태에 대한 내구성 있는 저장소와 상호 작용한다. 
- 일정 시간 동안 레코드를 재생할 수 있는 영구 (또는 지속) 데이터 원본이다. 영구 메시지 대기열(예: Apache Kafka, RabbitMQ, Amazon Kinesis, Google PubSub) 또는 파일 시스템(예: HDFS, S3, GFS, NFS, Ceph 등)이 있다.
- 상태용 영구 스토리지, 일반적으로 분산 파일 시스템(예: HDFS, S3, GFS, NFS, Ceph 등)

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
