# 클러스터 API 카테고리와 메타데이터 API 카테고리

## 클러스터 API
- 보안, 쿼터 등
```
노드(node)
네임스페이스(namespace)
영구 볼륨
리소스 쿼터(Resource Quater)
서비스 어카운트(Service account)
롤(Roll)
클러스터 롤(Cluster Roll)
롤바인딩(roll binding)
클러스터롤바인딩 (Cluster binding)
네트워크 정책
```

## 메타데이터 API
- 클러스터 내부 다른 리소스 관리하기 위함
```
LimitRange
HorizontalPodAutoscaler
PodDisruptionBudget
커스텀 리소스 데피니션(Custom resource definition)
```

## 노드
- 사용자가 생성/삭제 하는 리소스가 아니지만, 리소스로 등록한다.
- 쿠버네티스는 컨테이너를 파드내에 배치하고, 노드에서 실행함으로 워크로드를 구동한다.
- 노드는 클러스터에 따라 가상 또는 물리적 머신일 수 있다. 각 노드는 컨트롤 플레인에 의해 관리되며 파드를 실행하는 데 필요한 서비스를 포함한다.
- 일반적으로 클러스터에는 여러 개의 노드가 있으며, 학습 또는 리소스가 제한되는 환경에서는 하나만 있을 수도 있다.
```
// 노드 리소스 확인
kubectl get nodes {} -o yaml // 프로그래밍 내부에서 처리하는 경우
status.allocable
> 쿠버네티스가 시스템 리소스로 할당한 만큼 빼고, 실제 파드에 할당 가능한 리소스

status.nodeInfo
> 노드 버전 확인

// 현재 리소스 사용량
kubectl describe node {} //터미널에서 확인하는 경우
```

## 네임스페이스
- 가상의 쿠버네티스 클러스터 분리 기능
- 리소스의 이름은 네임스페이스 내에서 유일해야 하며, 네임스페이스 간에서 유일할 필요는 없다. 
- 네임스페이스는 여러 개의 팀이나, 프로젝트에 걸쳐서 많은 사용자가 있는 환경에서 사용하도록 만들어졌다. 
- 기본: default/kube-system/kube-public/kube-node-lease
```
default 쿠버네티스에는 이 네임스페이스가 포함되어 있으므로 먼저 네임스페이스를 생성하지 않고도 새 클러스터를 사용할 수 있다.
kube-node-lease 네임스페이스는 각 노드와 연관된 리스 오브젝트를 갖는다. 노드 리스는 kubelet이 하트비트를 보내서 컨트롤 플레인이 노드의 장애를 탐지할 수 있게 한다.
kube-public 네임스페이스는 모든 클라이언트(인증되지 않은 클라이언트 포함)가 읽기 권한으로 접근할 수 있다. 이 네임스페이스는 주로 전체 클러스터 중에 공개적으로 드러나서 읽을 수 있는 리소스를 위해 예약되어 있다. 이 네임스페이스의 공개적인 성격은 단지 관례이지 요구 사항은 아니다.
kube-system 쿠버네티스 시스템에서 생성한 오브젝트를 위한 네임스페이스이다.
```
- 리소스 인증 설정을 범위하거나, 에코시스템 도입 시에 많이 사용
```
kubectl create namespace sample-namespace
kubectl get pods -n sample-namespace
```

https://kubernetes.io/ko/docs/concepts/architecture/nodes/
