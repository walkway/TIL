# kubernetes

## Desired State
````
Loop
상태 체크: 현재 상태 == 원하는 상태
차이점 발견: 현재 상태 != 원하는 상태
조치: 현재 상태 -> 원하는 상태
````

## Master

### etcd
- 모든 상태 데이터 저장
- 분산 시스템으로 구성하여 안전성 높임
- key-value 형태 데이터 저장
- 백업 필수

### API server
- 상태를 바꾸거나 조회
- etcd 통신 모듈
- REST API 형태
- 권한 체크

### Scheduler
- 어떤 노드에 어떤 컨테이너를 실행할지
- 새로 생성된 Pod을 감지하고 실행할 노드 선택
- 노드의 현재 상태 Pod 요구사항 체크

### Controller
- 논리적으로 다양한 컨트롤러 존재하여 끊임 없이 상태를 체크하고 원하는 상태를 유지

## Node

### kubelet
- 각 노드에서 실행되어 Pod을 실행/중지하고 상태 체크

### proxy
- 내/외부 통신 설정
````
1. API Server로 Pod 생성 요청
2. etcd에 Pod 생성 요청이 들어왔다는 것을 기록
3. Controller는 새로 생긴 Pod이 있는지 주기적으로 체크
4. Controller는가 Pod 생성 요청을 확인하면, API server를 통해 Pod 할당 요청을 보냄
5. API server는 etcd에 Pod 할당 요청하라고 상태를 변경
6. Scheduler는 할당 요청된 Pod이 있는지 주기적 체크하고, 특정 노드에 Pod 할당
7. API Server는 etcd에 Pod - 노드 할당 / 미실행 기록
8. Kubelet은 미실행 Pod 주기적으로 확인하고 Pod 생성
9. API Server에 Pod 실행 업데이트 요청하고 etcd Pod - 노드 할당 / 실행 중 기록
````

## 오브젝트

### Pod
- 가장 작은 배포 단위, 고유한 IP 할당
- 여러개의 컨테이너가 하나의 Pod에 속할 수 있음

### ReplicaSet
- 여러개의 Pod 관리
- 신규 Pod 생성하거나 기존 Pod 제거하여 원하는 수 유지

### Deployment
- 배포 버전 관리

### Service - Cluster IP
- 클러스터 내부에서 사용하는 프록시
- Cluster IP(고정)로 요청하면 자동으로 Pod 3개중 1개로
- 내부에서만 사용

### Service - NodePort
노드(host)에 노출되어 외부에서 접근 가능한 서비스

### Service - LoadBalancer
- 하나의 IP주소를 외부에 노출

### Ingress
- 도메인, 경로별 라우팅

### Namespace
- 논리적인 리소스 구분

## API 호출
- 원하는 상태를 다양한 오브젝트로 정의하고 API 서버에 yaml 형식으로 전달

https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
https://medium.com/@jwlee98/gcp-gke-%EC%B0%A8%EA%B7%BC-%EC%B0%A8%EA%B7%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-1%ED%83%84-gke-%EA%B0%9C%EC%9A%94-382dc69b2ec4
https://www.youtube.com/watch?v=SNA1sSNlmy0

