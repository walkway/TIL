# 도커 
- 기존 가상화 기술은 하이퍼바이저를 이용, 여러 운영체제를 하나의 호스트에서 생성하고 사용하는 방식 > 가상 머신
- 하이퍼 바이저에 의해 생성되고 관리되는 운영체제 -> 게스트 운영체제
  - 시스템 자원을 가상화하고 독립된 공간 생성 작업은 하이퍼바이저를 거치기 때문에 일반 호스트에 비해 성능 손실
  - 가상 머신 이미지를 애플리케이션으로 배포하기 부담스러움
- 도커 컨테이너는 가상 공간 생성을 위해 리눅스 자체 기능 chroot, 네임스페이스, cgroup 사용
  - 프로세스 단위 격리 환경, 성능 손실 거의 없음
- 도커 이미지: 여러 계층으로 된 바이너리 파일로 존재, 읽기 전용
- 도커 컨테이너: 이미지로 컨테이너를 생성하면, 이미지 목적에 맞는 파일이 들어 있는 파일 시스템과 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간이 생성

### 애플리케이션 개발, 배포 편의성
- 도커 컨테이너는 호스트 OS 위에서 실행되는 격리된 공간, 컨테이너 자체에 특별한 권한을 주지 않는 한 호스트 OS에 영향을 주지 않음
- 터널을 포함하지 않아서 이미지 크기가 크지 않음
- 중복 레이어를 재사용할 수 있음

# 쿠버네티스
### 쿠버네티스는 v1.20 이후 컨테이너 런타임으로 도커 사용 중단
- 기본(underlying) 런타임 중 하나인 도커는 쿠버네티스의 컨테이너 런타임 인터페이스(CRI)를 사용하는 런타임으로써는 더 이상 사용되지 않음(deprecated)
  - kubernetes에서 dockershim 변환하는데 비효율적이라고 판단
- 도커가 생성한 이미지는 모든 런타임을 통해 클러스터에서 계속 작동
- https://kubernetes.io/ko/blog/2020/12/02/dont-panic-kubernetes-and-docker/

### Container Runtime
- OCI(Open Container Initiative): 컨테이너 런타임에 대한 표준으로 컨테이너를 실행하기 위한 저수준 런타임
- CRI(Container Runtime Interface): 쿠버네티스에서 제공하는 컨테이너 런타임 추상화 계층
- 저수준 런타임: namespace와 cgroup을 이용해서 컨테이너 자체를 만듬, 이미지로부터 컨테이너를 실행하는 기능 없음, runc
- 고수준 런타임: 저수준 런타임 위에 배치되어 이미지로부터 컨테이너 실행 가능, containerd, cri-o
- dockerd: Docker Daemon으로 REST API를 제공하며, Client로부터 컨테이너 관련 요청을 받는다. 대부분은 containerd로 위임하지만, 일부 작업(이미지 빌드, 네트워킹 등)은 직접 수행. 기본값은 Unix Domain Socket, TCP 방식으로 변경 가능
- containerd: High Level Container Runtime 기능을 담당하며 Daemon 방식으로 REST API를 제공
- runc: Low Level Container Runtime 기능을 담당. runc의 원래 역할인 컨테이너 생성 후 바로 종료
- containerd-shim: runc는 컨테이너를 생성한 후 즉시 종료되므로 컨테이너 안에서 생성되는 프로세스의 부모가 될 프로세스가 필요

### 장점
- 서버 자원 클러스터링, 마이크로서비스 구조의 컨테이너 배포, 서비스 장애 복구 기능 지원
  - 컨테이너: 환경과 상관없이 실행을 위해 필요한 모든 요소를 포함하는 소프트웨어 패키지 
- 구글, 레드햇을 비롯한 많은 오픈소스 진영에서 쿠버네티스의 소스코드에 기여하고 있어 성능과 안정성 면에서 신뢰받고 있음
- Persistent Volume, 스케줄링, 장애 복구, 오토 스케일링, 서비스 디스커버리 및 인그레스 등 컨테이너 기반의 클라우드 운영시 필요한 대부분의 기능과 컴포넌트를 사용자가 직접 커스터마이징
- CNCF(Cloud Native Computing Foundation)및 다른 클라우드 운영 도구들과 쉽게 연동되어 높은 확장성
  - 클라우드의 장점을 최대한 활용할 수 있도록 애플리케이션을 개발하고 또 구축하고 실행하는 방식
  - 데브옵스(DevOps)
  - 애플리케이션 운영 구조: 마이크로서비스
  - 애플리케이션 운영 인프라: 컨테이너
  - 지속적인 통합/배포(CI/CD): 애플리케이션을 더욱 짧은 주기로 고객에게 제공하는 자동화 프로세스
- Pod라는 객체를 통해 컴퓨팅 자원을 표준화
- Service와 Ingress를 통해 가상 네트워킹을 표준화
- Persistent Volume을 통해 스토리지, Service Account를 통해 실행 주체(Identity)를 표준화
````
불변의 실행환경(immutable infra)
애플리케이션 실행에 필요한 OS/Lib/Bin 모두 포함
애플리케이션의 빈번한 출시
무정지 서비스
초기 비용을 낮추고, 비즈니스 상황에 맞게 규모 조정
쿠버네티스와 외부 서비스와의 연동
개발환경과 운영환경의 분리
온프레미스와 클라우드 위에 구축
애플리켕션 중심의 오케스트레이션
특정 기업에 종속되지 않는 표준 기술
서버들의 가동률 향상
````
https://www.redhat.com/ko/topics/containers/what-is-kubernetes
https://www.samsungsds.com/kr/insights/docker.html
