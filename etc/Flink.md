# Flink
- 네이티브 스트림(Native Stream): 플링크는 일괄처리도 지원하지만 스트림 프로세싱을 주목적으로 사용한다. 경량의 분산 스냅샷을 구현하여 오버헤드는 낮으면서도 Exactly-once의 이벤트 처리를 보장할 수 있다.
- 인메모리(In-Memory): 자바 애플리케이션으로 JVM(Java Virtual Machine)에서 실행되지만 JVM GC(Garbage Collector)에 전적으로 의존하지 않는다. 대신 커스텀 메모리 매니저를 구현하여 안정적인 메모리 사용량을 유지하면서 성능을 향상한다.
- 낮은 지연과 높은 처리량(Throughput): 경쟁 제품에 비하여 지연과 처리량에서 우수한 성능을 보인다. 플링크는 데이터를 처리하는 과정에서 변경된 부분만 재처리하도록 설정하여 처리 속도를 더 높일 수도 있다.
- 손쉬운 설정과 사용 그리고 강력한 부가 기능: 스파크는 파라미터 설정이 복잡하지만 플링크는 별다른 설정 없이 사용할 수 있다. 입력되는 이벤트 스트림에 개별로 접근할 수 있고, 강력한 윈도우 연산자를 사용하여 분석을 수행할 수 있다. 고급 분석용 API부터 상세한 제어가 가능해지는 상태 저장 이벤트 기반 애플리케이션 수준까지 계층에 따른 API를 제공한다.
- 플링크에서 데이터는 소스(Source)로 시작해 싱크(Sink)로 끝난다. 데이터가 입력되어 처리가 완료되기까지 각 단계는 스트림으로 이동하고 오퍼레이터에 의하여 데이터가 처리된다. 소스는 데이터 입력을 정의하는 단계로 원천시스템의 로그, 클릭 이벤트, IoT 장치 등에서 발생하는 데이터를 실시간 이벤트 스트림이나 데이터베이스, 파일, 키-밸류 스토어(Key-Value Store) 수신된다.
- 트랜스포메이션(Transformation)은 데이터를 가공하는 작업이다. 스트림 내의 특정 값에 가중치를 주거나 분석하여 새로운 스트림을 형성할 수도 있고 특정 키(Key) 값 기반 스트림을 처리할 수도 있다. 스트림 처리 시스템의 기본 개념인 윈도우(Window) 관련 기능도 제공한다.

## Standalone
### session mode
- 여러 Flink Job을 실행, 클러스터가 배포된 후 각 Job을 클러스터에 Submit
- a Deployment which runs a JobManager
- a Deployment for a pool of TaskManagers
- a Service exposing the JobManager’s REST and UI ports
````
# Configuration and service definition
$ kubectl create -f flink-configuration-configmap.yaml
$ kubectl create -f jobmanager-service.yaml
# Create the deployments for the cluster
$ kubectl create -f jobmanager-session-deployment.yaml
$ kubectl create -f taskmanager-session-deployment.yaml
````

### appli
## Native Kubernetes
- 필요한 리소스에 따라 TaskManager를 동적으로 할당 및 해제할 수 있다.

````
// (1) Start Kubernetes session
$ ./bin/kubernetes-session.sh -Dkubernetes.cluster-id=my-first-flink-cluster

// (2) Submit example job
$ ./bin/flink run \
    --target kubernetes-session \
    -Dkubernetes.cluster-id=my-first-flink-cluster \
    ./examples/streaming/TopSpeedWindowing.jar

// (3) Stop Kubernetes session by deleting cluster deployment
$ kubectl delete deployment/my-first-flink-cluster
````

https://ci.apache.org/projects/flink/flink-docs-release-1.13/
https://www.samsungsds.com/kr/insights/flink.html