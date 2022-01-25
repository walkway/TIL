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

### Service 
파드 집합에서 실행중인 애플리케이션을 네트워크 서비스로 노출하는 추상화 방법
쿠버네티스를 사용하면 익숙하지 않은 서비스 디스커버리 메커니즘을 사용하기 위해 애플리케이션을 수정할 필요가 없다. 쿠버네티스는 파드에게 고유한 IP 주소와 파드 집합에 대한 단일 DNS 명을 부여하고, 그것들 간에 로드-밸런스를 수행할 수 있다.
쿠버네티스의 서비스는 파드와 비슷한 REST 오브젝트이다. 모든 REST 오브젝트와 마찬가지로, 서비스 정의를 API 서버에 POST하여 새 인스턴스를 생성할 수 있다. 서비스 오브젝트의 이름은 유효한 DNS 서브도메인 이름이어야 한다.
애플리케이션에서 서비스 디스커버리를 위해 쿠버네티스 API를 사용할 수 있는 경우, 서비스 내 파드 세트가 변경될 때마다 업데이트되는 엔드포인트를 API 서버에 질의할 수 ​​있다.
네이티브 애플리케이션이 아닌 (non-native applications) 경우, 쿠버네티스는 애플리케이션과 백엔드 파드 사이에 네트워크 포트 또는 로드 밸런서를 배치할 수 있는 방법을 제공한다.
````
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
````
이 명세는 "my-service"라는 새로운 서비스 오브젝트를 생성하고, app=MyApp 레이블을 가진 파드의 TCP 9376 포트를 대상으로 한다.
쿠버네티스는 이 서비스에 서비스 프록시가 사용하는 IP 주소 ("cluster IP"라고도 함) 를 할당한다. (이하 가상 IP와 서비스 프록시 참고)
서비스 셀렉터의 컨트롤러는 셀렉터와 일치하는 파드를 지속적으로 검색하고, "my-service"라는 엔드포인트 오브젝트에 대한 모든 업데이트를 POST한다.

### Service - Cluster IP
- 클러스터 내부에서 사용하는 프록시
- Cluster IP(고정)로 요청하면 자동으로 Pod 3개중 1개로
- 내부에서만 사용
- Kubernetes 기본 서비스로, 클러스터 내의 다른 앱이 접근할 수 있고, ClusterIP는 외부 접근이 되지 않음
````
apiVersion: v1
kind: Service
metadata:  
  name: my-internal-service
spec:
  selector:    
    app: my-app
  type: ClusterIP
  ports:  
  - name: http
    port: 80
    targetPort: 80
    protocol: TCP
````
예를 들어 다음처럼 접근할 수 있음
````
$ kubectl proxy --port=8080

http://localhost:8080/api/v1/proxy/namespaces//services/:/
http://localhost:8080/api/v1/proxy/namespaces/default/services/my-internal-service:http/
````
- Kubernetes 프록시를 사용해서 서비스에 접근해야 하는 경우 사용
  - 서비스를 디버깅하거나 어떤 이유로 노트북/PC에서 직접 접근할 때
  - 내부 대시보드 표시 등 내부 트래픽을 허용할 때
  - 이 방식에서는 권한 있는 사용자가 kubectl을 실행해야 하기 때문에, 서비스를 외부에 노출하는데 사용하거나 실서비스에서 사용하면 안됨

### Service - NodePort
- 노드(host)에 노출되어 외부에서 접근 가능한 서비스
- 내부망 연결, 클러스터 밖에 있지만 내부 네트워크 접근 전용
- 서비스에 외부 트래픽을 직접 보낼 수 있는 가장 원시적인(primitive) 방법
- 모든 Node(VM)에 특정 포트를 열어 두고, 이 포트로 보내지는 모든 트래픽을 서비스로 포워딩한다.
````
apiVersion: v1
kind: Service
metadata:  
  name: my-nodeport-service
spec:
  selector:    
    app: my-app
  type: NodePort
  ports:  
  - name: http
    port: 80
    targetPort: 80
    nodePort: 30036
    protocol: TCP
````
- 기본적으로 NodePort 서비스는 일반 ClusterIP 서비스와 다른 차이
  - 타입이 NodePort 이다.
  - node에 어떤 포트를 열어줄지 지정하는 nodePort라는 추가 포트가 있다. 이 포트를 지정하지 않으면, 아무 포트나 선택된다. 대부분의 경우 Kubernetes가 포트를 선택하도록 두어야 한다. 어떤 포트가 사용 가능한지에는 주의사항이 있다.
- 제약사항
  - 포트당 한 서비스만 할당할 수 있다.
  - 30000-32767 사이의 포트만 사용할 수 있다.
  - Node나 VM의 IP 주소가 바뀌면, 이를 반영해야 한다.
  - 이런 이유들 때문에 실서비스에서 이 방법으로 서비스를 직접적으로 노출시키는걸 추천하지 않는다. 데모 앱이나 임시로 사용한다.

### Service - LoadBalancer
- 하나의 IP주소를 외부에 노출
- LoadBalancer 서비스는 서비스를 인터넷에 노출하는 일반적인 방식이다. GKE에서는 Network Load Balancer를 작동시켜 모든 트래픽을 서비스로 포워딩하는 단 하나의 IP 주소를 제공한다.
- 서비스를 직접적으로 노출하기를 원한다면, LoadBalancer가 기본적인 방법일 것이다. 지정한 포트의 모든 트래픽은 서비스로 포워딩 될 것이다. 필터링이나 라우팅 같은건 없다. HTTP, TCP, UDP, Websocket, gRPC 등등 거의 모든 트래픽 종류를 보낼 수 있다.
- 단점은 LoadBalancer로 노출하고자 하는 각 서비스 마다 자체의 IP 주소를 갖게 된다는 것과, 노출하는 서비스 마다 LoadBalancer 비용을 지불해야 하기 때문에 값이 비싸진다는 것이다.

### Ingress
- 도메인, 경로별 라우팅
- Ingress는 서비스의 한 종류가 아니다. 여러 서비스들 앞에서 “스마트 라우터(smart router)” 역할을 하거나 클러스터의 진입점(entrypoint) 역할을 한다. 여러 능력을 가진 Ingress 컨트롤러 타입이 있어서, Ingress 하나만으로 여러 가지 일을 할 수 있다. 기본 GKE Ingress 컨트롤러는 HTTP(S) Load Balancer를 만들어 준다. 이것은 백엔드 서비스로 경로(path)와 서브 도메인 기반의 라우팅을 모두 지원한다. 예를 들어, foo.yourdomain.com으로 들어오는 모든 트래픽을 foo 서비스로 보낼 수 있고, yourdomain.com/bar/ 경로로 들어오는 모든 트래픽을 bar 서비스로 보낼 수 있다.
````
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: my-ingress
spec:
  backend:
    serviceName: other
    servicePort: 8080
  rules:
  - host: foo.mydomain.com
    http:
      paths:
      - backend:
          serviceName: foo
          servicePort: 8080
  - host: mydomain.com
    http:
      paths:
      - path: /bar/*
        backend:
          serviceName: bar
          servicePort: 8080
````
- Ingress는 서비스를 외부에 노출하는 가장 강력한 방법이면서, 복잡한 방법일 수 있다. Google Cloud Load Balancer와 Nginx, Contour, Istio 등과 같은 많은 Ingress 컨트롤러 타입이 있다. 그리고 SSL 인증서를 서비스에 자동으로 프로비저닝해 주는 cert-manager 같은 Ingress 컨트롤러 플러그인도 많다.
동일한 (보통 HTTP) L7 프로토콜을 사용하는 여러 서비스들을 같은 IP 주소로 외부에 노출한다면 Ingress가 가장 유용할 것이다. Native GCP integration을 사용한다면 단 하나의 로드 밸런서 비용만 지불하면 되고, ingress는 “스마트”하기 때문에 (SSL, Auth, Routing 과 같은) 생각지못했던 다양한 기능을 활용할 수 있다.

### Namespace
- 논리적인 리소스 구분
- 쿠버네티스는 동일 물리 클러스터를 기반으로 하는 복수의 가상 클러스터를 지원하는데, 이 가상 클러스터를 namespace라고 함
- 여러개의 팀이나 프로젝트에 걸쳐 많은 사용자가 있는 환경에서 사용

### ConfigMap
- key와 value로 구성된 오브젝트
- POD는 configmap을 환경변수로 사용 가능
````
apiVersion: v1
kind: ConfigMap
metadata:
  name: cm-dev
data:
  SSH: 'false'
  User: dev
````
- configMap은 많은 양의 데이터를 보유하도록 설계되지 않았다. configMap에 저장된 데이터는 1MiB를 초과할 수 없다. 이 제한보다 큰 설정을 저장해야 하는 경우, 볼륨을 마운트하는 것을 고려하거나 별도의 데이터베이스 또는 파일 서비스를 사용할 수 있다.
- configMap은 별도의 보안 또는 암호화를 제공하지 않는다. 저장되어 있는 data를 kubernetes api 또는 yaml 파일을 통해 손쉽게 값을 확인할 수 있어 Credential 정보를 저장하기에는 부적합하며, 일반적인 정보를 저장하는데 적합하다.
- configMap을 사용하여 Pod에서 참조할 수 있도록 구성하는 방법
  - 1) 컨테이너 내에서 CLI 또는 ARGS로 참조
  - 2) 컨테이너의 환경 변수 
  - 3) 애플리케이션이 참조할 수 있도록 읽기 전용 볼륨에 파일을 추가하여 참조
  - 4) 쿠버네티스 API를 사용하여 컨피그맵을 읽는 파드 내에서 실행할 코드 작성
- 1 ~ 3: kubelet이 Pod내 컨테이너를 시작할 때 configMap의 데이터 사용
- 4: configMap과 데이터를 읽기 위해 코드를 작성
````
// application.properties
log.level=${LOG_LEVEL}

//configMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: loglevel-configmap
data:
  loglevel: DEBUG

// deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
...
spec:
...
    spec:
      containers:
      - name: k8s-configmap
...
        env:
        - name: LOG_LEVEL
          valueFrom:
            configMapKeyRef: 
              name: loglevel-configmap
              key: loglevel
````
- properties 파일의 설정을 변경하면 소스코드 빌드/패키징 진행한 후 배포를 진행해야 했다
- configMap을 사용하면 kubectl edit configmap loglevel-configmap -> deployment restart
- configMap만 수정한 후에는 변경된 데이터가 반영되지는 않는다. configMap은 kubelet이 pod 내 container를 기동하는 시점에 configmap을 적용하기 때문이다. 다만 application level에서 수정하지 않고도 environment가 적용된다.

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

## service account
````
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
EOF
````
기본이 아닌 서비스 계정을 사용하려면 spec.serviceAccountName 팟(Pod) 의 필드를 사용하려는 서비스 계정의 이름으로 설정

## 노드에 포드 할당
````
kubectl label nodes <your-node-name> disktype=ssd
````
````
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
````
# Helm
- 쿠버네티스 패키지 매니저
- 하나의 애플리케이션을 설치하기 위한 파일로 구성
- 애플리케이션 컨테이너 배포, 이에 필요한 쿠버네티스 리소스를 모두 배포해주는 역할
- 클라이언트(cli)와 서버(쿠버네티스 클러스터에 설치되는 틸러)로 구성
  - 클라이언트: 서버를 대상으로 명령을 지시하는 역할
  - 서버: 클라이언트에서 전달받은 명령에 따라 쿠버네티스 클러스터에 패키지 설치, 업데이트, 삭제 등의 작업을 수행
````
//Helm 가이드
helm completion

//repo 검색
helm search repo [keyword]

//hub 검색
helm search hub [keyword]
 
//레포지토리 관리
helm repo add [name] [url]

//레포지토리 리스트 확인
helm repo list

//레포지토리 업데이트
helm repo update

//레포지토리 삭제
helm repo remove [name]
 
//배포 관리
helm install [name] [chart] [flags]

//배포 리스트 보기
helm list

//배포 상태 확인
helm status

//배포 삭제
helm uninstall
````

### 파일 역할
- Chart.yaml : 차트의 이름과 버전을 지정한다.
- Values.yaml : 사용자가 릴리즈 하려는 인스턴스에 대해 수정 가능한 설정을 포함한다.
- Templates 폴더 : values.yaml 파일에 선언된 변수를 이용해서 kubernetes 매니패스트 파일을 생성하는 템플릿 파일이 존재한다.
- Chart 폴더 : 의존성 있는 차트 패키지들 설치, requirement.yaml 파일에 의존성이 있는 차트를 명시할 경우, charts 폴더에 차트파일이 저장된다.

### hooks
- 개발자가 릴리스 수명주기의 특정 지점에 개입할 수 있도록 hook 매커니즘을 제공한다.
- 예시
  - 설치 시 다른 차트가 로드되기 전에 컨피그맵이나 시크릿을 로드한다.
  - 새 차트를 설치하기 전에 데이터베이스를 백업하는 작업을 실행하고, 데이터 복원을 위해 업그레이드 후 2번째 작업을 수행한다.
  - 릴리스를 제거하기 전에 서비스를 순환에서 안전하게 제거하기 위하여, 릴리스 삭제 전에 작업을 수행한다.
````
annotations:
  "helm.sh/hook": pre-install
````

https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
https://medium.com/@jwlee98/gcp-gke-%EC%B0%A8%EA%B7%BC-%EC%B0%A8%EA%B7%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-1%ED%83%84-gke-%EA%B0%9C%EC%9A%94-382dc69b2ec4
https://www.youtube.com/watch?v=SNA1sSNlmy0
https://bcho.tistory.com/1337
https://waspro.tistory.com/681
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
