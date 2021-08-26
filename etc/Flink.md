# Flink

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