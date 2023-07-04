# 유연한 고급 스케줄링
## 필터링과 스코어링
- 필터링: 스케줄링하는 파드가 할당 가능한 노드를 계산
  - 스케줄링하는데 충분한 리소스가 있는지, 필수 조건으로 지정한 레이블을 가진 노드인지 등을 체크
- 스코어링: 필터링 후 노드 목록에 순위를 매겨 적합한 노드 계산
  - 디플로이먼트를 최대한 분산하여 노드에 배치하거나 이미 사용하는 컨테이너 이미지가 있는 노드를 우선 처리하거나 등
- 필터링, 스코어링이 같은 경우 무작위 선택

## 매니페스트에서 지정하는 스케줄링
- 쿠버네티스 사용자가 배치하고 싶은 노드를 선택하는 방법
- 쿠버네티스 관리자가 배치하고 싶지 않은 노드를 지정하는 방법

## 빌트인 노드 레이블과 레이블 추가
- 빌트인 노드: 최소한 호스트명/OS/아키텍처/인스턴스 타입/리전/존 정보 부여
kubectl get nodes -o json | jq ".items[]" | .meta.labels"

- 수동으로 노드에 레이블 부여: ex 디스크 종류, cpu 속도, cpu 세대 등 가정
````
kubectl label node {node명} disktype=hdd cpuspec=low cpugen=3

// 확인
kubectl -L=disktype,cpuspec,cpugen get node
````

## nodeSelector
- 단순한 노드 어피니티만 실행하는 경우, 다중 조건 불가
- ex 높은 디스크 IO 성능이 필요한 파드를 배치하고 싶은 경우
````
nodeSelector:
   disktype: sdd
````

## 노드 어피니티
- 파드를 특정 노드에 스케줄링 하는 정책
- sepc.affinity.nodeAffinity
  - requiredDuringSchedulingIgnoredDuringExecution: 필수 스케줄링 정책
  - 필수 조건이 만족하는 경우에 우선적 고려되는 정책을 확인
  - 다음처럼 정의 한 경우 (A and B) or (C and D)
````
nodeSelectorTerms:
   - matchExpressions:
     - A
     - B
   - matchExpressions:
     - C
     - D
````
  - preferredDuringSchedulingIgnoredDuringExecution: 우선적으로 고려되는 스케줄링 정책
    - weight 가중치를 줄 수 있음
    - matchExpressions 조건

## matchExpressions 오퍼레터와 집합성 기준 조건
- matchExpressions: key 레이블, 오퍼레이터, values 레이블 (배열)
````
// 오퍼레이터
IN: 하나 이상 일치
NotIn: 어느 것도 일치하지 않음
Exists: 존재, values 레이블 지정할 수 없음
DoesNotExist: 존재하지 않음,  values 레이블 지정할 수 없음
Gt: A 레이블 값이 B보다 큼, values 하나의 정수
Lt: A 레이블 값이 B보다 작음, values 하나의 정수
````
- matchExpressions는 레플리카셋, 디플로이먼트, 데몬셋, 스테이트풀셋, 잡 셀럭터로 사용

## 노드 안티어피니티
- 파드를 특정 노드 이외의 다른 노드에 스케줄링하는 정책
- 노드 어피니티의 sepc.affinity.nodeAffinity 조건 부분에 부정형 오퍼레이터를 지정하여 구현

## 인터파드 어피니티
- 특정 파드가 실행되는 도메인(노드, 존 등)에 파드를 스케줄링하는 정책
  - 존: 데이터 센터에서 전원, 기계들이 분리되어 있고 기본적으로 동시에 장애가 발생하지 않도록 물리적으로 분리되어 있는 영역
- sepc.affinity.podAffinity 조건 지정
- 파드끼리 가까이 배치할 수 있어서 파드 간의 통신 레이턴시를 낮출 수 있음
- 노드들의 기능 차이가 적은 경우 노드를 의식하지 않고 파드 기반의 스케줄링 만으로 전체 제어
- requiredDuringSchedulingIgnoredDuringExecution, preferredDuringSchedulingIgnoredDuringExecution
- toologyKey: 어느 범위(도메인)를 스케줄링 대상으로 할지 지정
p 535
- (A and B) 파드와 같은 X에 있는 노드 or (C and D) 파드와 같은 Y에 있는 노드
````
requiredDuringSchedulingIgnoredDuringExecution:
- labelSelector:
   matchExpressions:
   - A
   - B
  topologyKey: X
- labelSelector:
   matchExpressions:
   - C
   - D
  topologyKey: Y
````

## 인터파드 안티어피니티
- 특정 파드가 없는 도메인(노드, 존 등)에서 동작시키는 정책
- spec.affinity.podAntiAffinity 지정

## TopologySpreadConstraints를 사용한 토폴로지 균형
- 지정한 레이블을 가진 파드가 특정 토폴로지에 대해 어느정도 편차 허용 범위를 가지도록 설정하여, 스케줄링 제약
- ex 같은 존에 한 개 파드보다 많은 편차를 허용하지 않는 스케줄링
````
topologySpreadConstraints:
- topologyKey: topology.kubernetes.io/zone
  labelSelector:
    matchLabels:
      app: sample-app
  maxSkew: 1
  whenUnsatisfiable: DoNotSchedule
````

## 테인트와 톨러레이션
- 쿠버네티스 관리자가 배치하고 싶지 않은 노드를 지정하는 방법
- 노드에 대한 오염을 설정해두고 그것을 허용할 수 있는 파드만 스케줄링 허가 정책
- 조건에 맞지 않는 파드를 그 노드상에서 축출
- Key=Value:Effect 형식
````
// Effect
PreferNoSchedule: 가능한 스케줄링 하지 않음
NoSchedule: 스케줄링하지 않음, 이미 스케줄링된 파드는 그대로 유지
NoExecute: 실행을 허가하지 않음, 이미 스케줄링된 파드 정지
````

## 톨러레이션을 지정한 파드 기동
- Key/Value/Effect 지정하고 테인트에 부여된 Key/Value/Effect가 같은 경우에 조건이 일치한다고 판단하고 허용
- NoExecute 일정 시간 허용

## PrioirityClass를 이용한 파드 우선순위와 축출
- 파드가 이미 리소스의 한계까지 스케줄링된 상태에서 우선순위가 더 높은 파드를 생성하려는 경우 기존 파드를 축출시킬 수 있음
  - 축출 후, 어피니티 등 스케줄링 조건 만족하는 여부 판단 함. 만족하지 않는 경우 스케줄링 수행하지 않음
- value = 우선순위, 높을 수록 먼저 기동
````
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: sample-priority-class
value: 100
globalDefault: false
description: "used for serviceA only"
````
- 파드 정의 spec.podPriorityClass에 지정

## 스케줄링 성능 튜닝, 노드 스코어링 임계 값
- 스케줄러의 동작을 튜닝하여 응답 시간 (새 파드가 빠르게 배치됨)과 정확도(스케줄러가 배치 결정을 잘 못하는 경우가 드물게 됨) 사이에서의 스케줄링 결과를 균형 잡을 수 있습니다.
- percentageOfNodesToScore 기능은 kube-scheduler가 실행 가능한 노드가 충분히 발견하면 찾는 것을 중단함으로 시간을 절약합니다.
```
apiVersion: kubescheduler.config.k8s.io/v1alpha1
kind: KubeSchedulerConfiguration
algorithmSource:
  provider: DefaultProvider
...
percentageOfNodesToScore: 50
```
https://kubernetes.io/ko/docs/concepts/scheduling-eviction/scheduler-perf-tuning/
