# yarn
### ResourceManager
- 클러스터에 1개 존재하며, 클러스터의 전반적인 자원 관리와 스케쥴러와 Application Manager를 조정하는 역할을 한다.
- 클라이언트로부터 어플리케이션 실행 요청을 받으면, 그 실행을 책임질 Application Master를 실행한다.
- 클러스터 내의 Node Manager들과 통신을 해서 할당된 자원과 사용중인 자원의 상황을 알 수 있다.

### Scheduler
- Node Manager들의 자원 상태를 관리하며 부족한 리소스를 할당한다.
- 자원 상태에 따라서 태스크들의 실행 여부를 허가해주는 역할인 스케쥴링 작업만 담당한다.
- Node Manager들의 자원 상태를 Resource Manager에게 통지한다.

### Application Manager
- Node Manager에서 특정 작업을 위해 Application Manager를 실행하고, Application의 실행 상태를 관리하여 그 상태를 Resource Manager에게 통지한다.

### Node Manager
- 노드 당 한개씩 존재하며, Container의 리소스 사용량을 모니터링하고, 관련 정보를 Resource Manager에게 알리는 역할을 한다.

### Application Master
- YARN에서 실행되는 하나의 태스크를 관리하는 마스터 서버를 말한다. 어플리케이션 당 1개가 있다.
- Scheduler로부터 적절한 Container를 할당 받고, 프로그램의 실행 상태를 모니터링, 관리한다.
 
### Container
- CPU, Disk, Memory 등의 리소스로 정의된다.
- 모든 작업은 결국 여러개의 태스크로 세분화되고, 각 테스크는 하나의 Container에서 실행된다. 
- 필요한 자원의 요청은 Application Master가 담당하며, 승인 여부는 Resource Manager가 담당합니다. Container안에서 실행할 수 있는 프로그램은 자바프로그램뿐만 아니라, 커맨드 라인에서 실행할 수 있는 프로그램이라면 모두 가능하다.

````
Client가 Resource Manager에게 Application에 대한 요청을 전송한다.
Resource Manager는 Application Master와 함께 Application을 등록한다. (이때 Application ID가 생성되며, 후에 Client에게 반환된다.)
Resource Manager는 각각의 분리된 Container에서 Application Master를 구동한다. 만약 Container가 구동 불가능하다면, 적합한 Container를 찾을 때까지 기다린다.
Application Master는 Node Manager에게 Container의 실행 명령을 전달한다.
Application 코드가 Container에서 실행된다.
Client는 Application의 상태를 모니터링하기 위해 Resource Manager/Application Manager와 주고 받는다.
Application Master가 Resource Manager에서 등록 해지된다.
````