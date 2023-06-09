# Spark

- 인메모리 기반의 대용량 데이터 고속 처리 엔진
- Speed: 인메모리(In-Memory) 기반의 빠른 처리
  - 맵리듀스는 작업의 중간 결과를 디스크에 씀, IO로 인하여 작업 속도 제약
  - 중간 결과를 메모리에 저장하여 반복 작업의 처리 효율이 높음
- Ease of Use: 다양한 언어 지원(Java, Scala, Python, R, SQL)
- Generality: SQL, Streaming, 머신러닝, 그래프 연산 등
- Run Everywhere: YARN, Mesos, Kubernetes 등 다양한 클러스터에서 동작
- 스파크 라이브러리, 스파크 코어, 클러스터 매니저
- 드라이버: 스파크 컨텍스트 객체를 생성하여 클러스터 매니저와 통신하면서 클러스터의 자원 관리를 지원하고, 애플리케이션의 라이프 사이클을 관리
- 클러스터 매니저: 작업을 관리하는 드라이버와 작업이 실행되는 노드를 관리
- driver
  - 스파크 애플리케이션을 실행하는 프로세스
  - 스파크 컨텍스트(SparkContext) 객체를 생성
  - 클라이언트 모드는 클러스터 외부에서 드라이버를 실행하고, 클러스터 모드는 클러스터 내에서 드라이버를 실행
- executor
  - task 실행을 담당하는 에이전트로 실제 작업을 진행하는 프로세스
  - task 단위로 작업을 실행하고 결과를 드라이버에 알려줌

- Task
  - 실행되는 작업

## Deployment
### Standalone Deployment
- Hadoop 클러스터의 모든 시스템 또는 하위 시스템에 Resource를 동적으로 할당하고, Hadoop의 MapReduce와 Spark 실행
- HDFS 데이터에서 임의의 Spark 작업을 실행 가능

### Hadoop Yarn Deployment
- 이미 Hadoop Yarn을 배포했거나 배포하려는 사용자는 사전 설치나 관리 엑세스 없이 Spark 실행 가능
