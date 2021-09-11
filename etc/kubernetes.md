# kubernetes
- 컨테이너화된 워크로드와 서비스를 관리하기 위해 이식성, 확장 가능한 오픈 소스 플랫폼
- 컨테이너 오케스트레이션 툴
- 컨테이너의 생성, 소멸, 시작, 중단 시점 제어, 스케줄링, 로드 밸런싱, 클러스터링 등 컨테이너로 어플리케이션을 구성하는 과정 관리

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
- 쿠버네티스 시스템에서 영속성을 가지고, 클러스터의 상태를 나타냄
- 어떤 컨테이너화된 어플리케이션이 동작 중인지
- 어플리케이션이 이용할 수 있는 리소스
- 재구동 정책, 업그레이드, 내고장성 동작 정책

### Pod
- 가장 작은 배포 단위, Running Process, 고유한 IP 할당
- namespace는 pod들을 연결해주고 각각 IP를 할당 함, 서로 다른 namespace의 pod들은 연결할 수 없음
- 여러개의 컨테이너가 하나의 Pod에 속할 수 있음, 하나 이상의 컨테이너의 그룹
- pod의 콘텐츠는 항상 함께 배치되고, 함께 스케줄되며, 공유 콘텍스트에서 실행
- 도커 개념 측면에서, 파드는 공유 네임스페이스와 공유 파일시스템 볼륨이 있는 도커 컨테이너 그룹과 비슷

#### 라이프 사이클
- Pending 단계에서 시작해서, 기본 컨테이너 중 적어도 하나 이상이 OK로 시작하면 Running 단계를 통과하고, 그런 다음 파드의 컨테이너가 실패로 종료되었는지 여부에 따라 Succeeded 또는 Failed 단계로 이동한다.
파드가 실행되는 동안, kubelet은 일종의 오류를 처리하기 위해 컨테이너를 다시 시작할 수 있다. 파드 내에서, 쿠버네티스는 다양한 컨테이너 상태를 추적하고 파드를 다시 정상 상태로 만들기 위해 취할 조치를 결정한다.
- 쿠버네티스 API에서 파드는 명세와 실제 상태를 모두 가진다. 파드 오브젝트의 상태는 일련의 파드 조건으로 구성된다. 사용자의 애플리케이션에 유용한 경우, 파드의 조건 데이터에 사용자 정의 준비성 정보를 삽입할 수도 있다. 파드는 파드의 수명 중 한 번만 스케줄된다. 파드가 노드에 스케줄(할당)되면, 파드는 중지되거나 종료될 때까지 해당 노드에서 실행된다.
- 개별 애플리케이션 컨테이너와 마찬가지로, 파드는 비교적 임시(계속 이어지는 것이 아닌) 엔티티로 간주된다. 파드가 생성되고, 고유 ID(UID)가 할당되고, 종료(재시작 정책에 따라) 또는 삭제될 때까지 남아있는 노드에 스케줄된다. 만약 노드가 종료되면, 해당 노드에 스케줄된 파드는 타임아웃 기간 후에 삭제되도록 스케줄된다.
- 파드는 자체적으로 자가 치유되지 않는다. 파드가 노드에 스케줄된 후에 해당 노드가 실패하면, 파드는 삭제된다. 마찬가지로, 파드는 리소스 부족 또는 노드 유지 관리 작업으로 인한 축출에서 살아남지 못한다. 쿠버네티스는 컨트롤러라 부르는 하이-레벨 추상화를 사용하여 상대적으로 일회용인 파드 인스턴스를 관리하는 작업을 처리한다.
- UID로 정의된 특정 파드는 다른 노드로 절대 "다시 스케줄"되지 않는다. 대신, 해당 파드는 사용자가 원한다면 이름은 같지만, UID가 다른, 거의 동일한 새 파드로 대체될 수 있다.
- 볼륨과 같은 어떤 것이 파드와 동일한 수명을 갖는다는 것은, 특정 파드(정확한 UID 포함)가 존재하는 한 그것이 존재함을 의미한다. 어떤 이유로든 해당 파드가 삭제되고, 동일한 대체 파드가 생성되더라도, 관련된 그것(이 예에서는 볼륨)도 폐기되고 새로 생성된다.

### 컨테이너 재시작 정책
- 파드의 spec 에는 restartPolicy 필드가 있다. 사용 가능한 값은 Always, OnFailure 그리고 Never이다. 기본값은 Always이다.
- restartPolicy 는 파드의 모든 컨테이너에 적용된다. restartPolicy 는 동일한 노드에서 kubelet에 의한 컨테이너 재시작만을 의미한다. 파드의 컨테이너가 종료된 후, kubelet은 5분으로 제한되는 지수 백오프 지연(10초, 20초, 40초, …)으로 컨테이너를 재시작한다. 컨테이너가 10분 동안 아무런 문제없이 실행되면, kubelet은 해당 컨테이너의 재시작 백오프 타이머를 재설정한다.

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
- 노드(host)에 노출되어 외부에서 접근 가능한 서비스
- 내부망 연결, 클러스터 밖에 있지만 내부 네트워크 접근 전용

### Service - LoadBalancer
- 하나의 IP주소를 외부에 노출

### Ingress
- 도메인, 경로별 라우팅

### Namespace
- 논리적인 리소스 구분
- 쿠버네티스는 동일 물리 클러스터를 기반으로 하는 복수의 가상 클러스터를 지원하는데, 이 가상 클러스터를 namespace라고 함
- 여러개의 팀이나 프로젝트에 걸쳐 많은 사용자가 있는 환경에서 사용

### ConfigMap
- key와 value로 구성된 오브젝트
````
apiVersion: v1
kind: ConfigMap
metadata:
  name: cm-dev
data:
  SSH: 'false'
  User: dev
````

### Secret
- 보안이 필요한 vaule를 저장하는 오브젝트로 메모리에 저장
````
apiVersion: v1
kind: Secret
metadata:
  name: sec-dev
data:
  Key: MTIzNA== #base64 encoding
````

- Pod 생성시 ConfigMap, Secret
````
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
spec:
  containers:
    - name: container
      image: tmkube/init
      envFrom:
        - configMapRef:
            name: cm-dev
        - secretRef:
            name: sec-dev
````

## API 호출
- 원하는 상태를 다양한 오브젝트로 정의하고 API 서버에 yaml 형식으로 전달

# Helm
- 쿠버네티스 패키지 매니저
- 하나의 애플리케이션을 설치하기 위한 파일로 구성
- 애플리케이션 컨테이너 배포, 이에 필요한 쿠버네티스 리소스를 모두 배포해주는 역할
- 클라이언트(cli)와 서버(쿠버네티스 클러스터에 설치되는 틸러)로 구성
  - 클라이언트: 서버를 대상으로 명령을 지시하는 역할
  - 서버: 클라이언트에서 전달받은 명령에 따라 쿠버네티스 클러스터에 패키지 설치, 업데이트, 삭제 등의 작업을 수행

https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
https://medium.com/@jwlee98/gcp-gke-%EC%B0%A8%EA%B7%BC-%EC%B0%A8%EA%B7%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-1%ED%83%84-gke-%EA%B0%9C%EC%9A%94-382dc69b2ec4
https://www.youtube.com/watch?v=SNA1sSNlmy0
https://bcho.tistory.com/1337