# RocksDB State Backend

### state backend
- Data Stream API 로 작성된 프로그램은 다양한 형태로 상태를 유지한다.
  - Windows는 트리거될 때까지 요소 또는 집계를 수집한다.
  - 변환 함수는 값을 저장하기 위해 키/값 상태 인터페이스를 사용한다.
  - 변환 함수는 CheckpointedFunction 인터페이스를 구현하여 로컬 변수를 fault tolerant으로 만들 수 있다.
  - 체크포인트가 활성화되면, 상태가 체크포인트에서 지속되어 데이터 손실을 방지하고 일관되게 복구 한다. 
  - HashMapState(default), EmbeddedRocksDBStateBackend

### EmbeddedRocksDBStateBackend
- EmbeddedRocksDBStateBackend 는 (기본적으로) TaskManager 로컬 데이터 디렉토리에 저장된 RocksDB 데이터베이스에 in-flight 데이터를 보유한다.
- Jobs with very large state, long windows, large key/value states.
- All high-availability setups.
- RocksDB를 클러스터에서 실행하고 전문 관리자가 관리해야 하는 분산 데이터베이스로 생각하는 것은 일반적인 오해이다. RocksDB는 빠른 저장을 위한 내장형 영구 키-값 저장소이다.
````
state.backend: rocksdb
state.checkpoints.dir: file:///checkpoint-dir/

# Optional, Flink will automatically default to FileSystemCheckpointStorage
# when a checkpoint directory is specified.
state.checkpoint-storage: filesystem
````
````
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
env.setStateBackend(new EmbeddedRocksDBStateBackend());
````

https://flink.apache.org/2021/01/18/rocksdb.html
https://nightlies.apache.org/flink/flink-docs-master/docs/ops/state/state_backends/
