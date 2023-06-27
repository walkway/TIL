# 메인터넌스와 노드 정지

## 스케줄링 대상에서 제외와 복귀(cordon/uncordon)
- 노드는 SchedulingEnabled, SchedulingDisabled 상태를 가짐
- SchedulingEnabled: 기본 상태
````
kubectl uncordon {node명}
````
- SchedulingDisabled: 스케줄링 대상에서 제외되어 노드상에 파드가 신규로 생성되지 않음 / 이후 스케줄링 대상에서 제외되고, 노드에서 이미 실행 중인 파드는 정지되지 않음
````
kubectl cordon {node명}
````

### 노드 배출 처리로 인한 파드 축출(drain)
- 노드상에 실행 중인 모든 파드를 축출시키는 배출처리를 하는 경우 사용
````
kubectl drain {node명}
````
- 노드를 SchedulingDisabled 상태로 변경하고각 파드에 SIGTERM 신호를 보내서 파드를 축출
````
// 디플로이먼트 등으로 관리되지 않는 파드 삭제할 때
--force

// 로컬 스토리지를 사용하는 파드 삭제할 때
--delete-local-data

// 데몬셋 관리하는 파드 삭제할 때
--ignore-daemonsets
````

## PodDistruptionBudget(PDB)을 사용한 안전한 축출
- 노드가 배출 처리를 할 때 파드를 정지할 수 있는 최대 수를 제한하는 리소스
- 파드가 축출될 때 특정 디플로이먼트 관리하에 있는 레플리카가 동시에 모두 정지되면 다운타임이 발생할 수 있음
- PodDistruptionBudget을 설정하면 조건에 일치하는 파드의 최소 기동 수(spec.minAvailable), 최대 정지 수(spec.maxUnavailable)을 보면서 노드상에서 파드 축출
  - 백분율로 설정 가능
