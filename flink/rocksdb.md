# RocksDB State Backend

### state backend
- Data Stream API 로 작성된 프로그램은 다양한 형태로 상태를 유지
  - Windows는 트리거될 때까지 요소 또는 집계를 수집
  - 변환 함수는 값을 저장하기 위해 키/값 상태 인터페이스를 사용
  - 변환 함수는 CheckpointedFunction 인터페이스를 구현하여 로컬 변수를 fault tolerant으로 만들 수 있음
  - 체크포인트가 활성화되면, 상태가 체크포인트에서 지속되어 데이터 손실을 방지하고 일관되게 복구 
  - HashMapState(default), EmbeddedRocksDBStateBackend
- Flink state -> 진행 중인 상태, 상태 스냅샷
  - 진행 중 상태: 작업 상태, 로컬 메모리에 저장, 작업이 실패하면 작업 복구 가능성에 영향을 주지 않고 손실될 수 있음
  - 상태 스냅샷: 체크포인트 / 세이브 포인트, 내구성 있는 원격 스토리지에 저장, 작업 실패 시 로컬 상태를 복원하는데 사용
  - state backend는 확장성, 처리량, 대기 시간에 따라 선택

### RocksDB
- 빠른 저장을 위한 내장형 영구 key-value 저장소
- JNI(Java Native Interface) 통해서 Flink와 상호 작용
- runtime에 TaskManager 프로세스에 포함
- native thread 에서 실행, 로컬 파일과 작용
- RocksDB 사용 이유
  - in-flight state가 off heap/native memory에 기록되고, 구성된 임계 값에 도달하면 로컬 디스크로 플러시
  - 일반적으로 원격 디스크에 쓰는 것은 느리기 때문에 NFS, HDFS 같은 원격 네트워크 기반 위치에 구성하지 않은 것이 좋음
  - 전체 클러스터에서 사용 가능한 디스크 양에 제한
  - JVM heap은 사용하지 않기 때문에 GC 영향 받지 않음
  - incremental 체크포인트 지원
  - 긴 window, 큰 key 상태를 사용할 때
  - key/value 쌍을 직렬화된 바이트로 저장하여, heap 기반 state backend보다 느림

### EmbeddedRocksDBStateBackend
- EmbeddedRocksDBStateBackend 는 (기본적으로) TaskManager 로컬 데이터 디렉토리에 저장된 RocksDB 데이터베이스에 in-flight 데이터를 보유
- Jobs with very large state, long windows, large key/value states.
- All high-availability setups.
- RocksDB를 클러스터에서 실행하고 전문 관리자가 관리해야 하는 분산 데이터베이스로 생각하는 것은 일반적인 오해. RocksDB는 빠른 저장을 위한 내장형 영구 키-값 저장소
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

### flink 1.16
- 이전에는 RocksDB의 로그가 자체 DB 폴더에 위치하여 RocksDB 디버깅이 쉽지 않았음
- 1.16에서는 RocksDB의 로그가 기본적으로 Flink의 로그 디렉토리에 유지.
- RocksDB 통계 기반 지표는 데이터베이스 수준에서 성능 디버깅을 돕기 위해 도입(예: DB 내의 총 블록 캐시 적중/실패 수)

https://flink.apache.org/2021/01/18/rocksdb.html
https://nightlies.apache.org/flink/flink-docs-master/docs/ops/state/state_backends/
https://flink.apache.org/news/2022/10/28/1.16-announcement.html
