# Kubernetes
- facebook k8skr

## Everything on Kubernetes: kubernates와 같이 쓰는 도구(?) 설명
- crossplane: yaml 파일을 통해서 클라우드 리소스(ex. S3)를 만들어주는 도구
  - https://www.crossplane.io/
- ClusterApi
- VM in 클러스터
  - 쿠버네티스 위에 VM을 올리기 위해
  - https://kubevirt.io/
- Knative: kubernetest위에 서버리스 플랫폼을 만들어 줌
- CI: 클라우드 위에서 올라갈 수 있는 CI도구
  - ex 젠킨스는 cloudnative하지 않아서 어려울 수 있다?
- CD: argo
- DB
  - dok.community/
  - vitess and slack: https://slack.engineering/scaling-datastores-at-slack-with-vitess/
  - 위험하지 않을까? 네트워크 성능, 스토리지 I/O 성능 문제 없을 정도로 / 리소스 효율적인 활용 가능 
- Security: Kyverno

## Kubernetes 기반의 앱 배포 시스템 만들기
- node labeling
  - nodeSelector
  - nodeAffinity
  - nodeAntiAffinity
- 요청을 안보내도 hang걸린 것을 아는 방법?
  - 기존엔.. log를 계속 보는 것(안 찍히니까)
  - 쿠버네티스에서는 Liveness, Readiness, Startup probe
- Controller: Control Loop (Not terminated Loop), Reconcilation
  - desired state, current state를 맞추기위해
- ConfigMap & Secret
  - pod 내에서 읽어올 수 있음
  - image내에 설정이 있으면 immutable하니, configmap을 활용하는 것이 괜찮음
- service
  - pod에 접근하는 변하지 않는 값
- pod resource 산정 관련해서
  - java는 nio를 쓰는데, heap 메모리 잡은것만 쓰지 않고 os 영역 메모리도 쓴다.
  - heap 메모리 외에 쓰는 것도 있으니, oom이 날 수 있다. 적절하게 조금더 여유 있게 해줘야 함 / limit을 주지 않던지

## ClusterAPI
- 쿠버네티스 클러스터를 프로비저닝, 업그레이드 및 운영하기 위한 선언적 API 및 도구를 제공하는 쿠버네티스 하위 프로젝트
- 장점
  - 선언적 관리 -> GitOps
  - 컨트롤러에 의해 지속적으로 관리
  - 멀티클라우드 환경에서 일관된 방식으로 쿠버네티스 클러스터 구축
- 단점
  - 상위 수준에서 일관된 API로 추상화하였지만, 각 인프라 프로바이더 별 자원의 내역 및 구현 동작에 대해서 이해 검증 필요
  - 모든 환경에서 인프라 프로바이더가 존재하지 않고 완성도에 차이가 있음
- 참고: https://devocean.sk.com/blog/techBoardDetail.do?ID=163908
