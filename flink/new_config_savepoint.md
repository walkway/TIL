# checkpoint, savepoint 의미 명확화
- 1.15 version release에서 도입
- checkpoint 목적: Flink에 의해 제어되고, 빠른 복구/장애 조치를 통해 내결함성(fault tolerance)을 보장하기 위한 것
- savepoint 목적: 사용자가 Flink 작업 전환, 백업/업그레이드를 제어하도록 하는 것
- 두 개념은 유사하고, 기본 구현도 동일한 아이디어 측면에서 만들어졌음, 때로 중요한 아이디어와 전략이 무시되는 경우가 있어, 이것을 사용자 피드백을 기반으로 조금 더 명확히 하는 작업을 진행 함
- Snapshots ownership을 checkpoint와 savepoint 사이 유일한 차이점으로 만드는 것을 목표로 함

### Savepoint format
사용자가 포맷은 선택할 수 있도록 함
- canonical format(기존 version에서 사용되던 방식): 모든 상태 백엔드에서 통합된 형식으로, 하나의 상태 백엔드로 저장점을 가져온 다음 다른 상태 백엔드를 사용하여 복원. 이것은 이전 버전, 스키마, 수정 사항 등과의 호환성을 최대한 유지하는 것을 목표로 하는 가장 안정적인 형식.
- native format(1.15에서 도입): canonical format 단점은 가져오고 복원하는 데 느리다는 것. canonical format 은 사용된 상태 백엔드에 특정한 형식으로 스냅샷을 생성(예: RocksDB용 SST 파일).

### Restore mode
savepoint 복원 후, 구성하는 파일의 ownership을 누가 가져갈지 결정 (Savepoint or externalized checkpoints)
사용자가 snapshot을 소유하는 경우 Flink는 해당 파일을 삭제하지 않음
````
$ bin/flink run -s :savepointPath -restoreMode :mode -n [:runArgs]
````
NO_CLAIM (default)
- Flink가 snapshot의 ownership을 인수하지 않음. 사용자의 제어에 파일을 남겨두고, 파일을 삭제하지 않음. 
- 동일한 snapshot에서 여러 Job을 시작할 수 있음.

CLAIM
- Flink는 snapshot의 ownership을 가지고, 이를 checkpoint처럼 취급
- 수명 주기를 제어하고 더 이상 복구에 필요하지 않은 경우 삭제할 수 있음.
- snapshot을 수동으로 삭제하거나 동일한 snapshot에서 두 Job을 시작하는 것은 안전하지 않음

LEGACY
- 1.15 vesrsion 이전까지 Flink가 작동했던 mode
- Flink가 초기 checkpoint를 삭제하지 않음
- 문제는 Flink가 복원된 checkpoint 위에 즉시 incremnetal checkpoint를 만들 수 있으므로 따라서 이후 checkpoint가 복원된 checkpoint에 따라 달라짐
- (참고: incremental checkpoint는 state backend를 RocksDB로 사용하는 경우, 전체 checkpoint에 비해 시간을 줄일 수 있는 방식, 전체 backup을 생성하는 대신 가장 최근 완료된 checkpoint 이후 발생한 변경 사항만 기록)
- 전반적으로 ownership이 명확하지 않음
