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

## Spark APIs: RDDs vs DataFrames and Datasets
### RDD
- Spark Core에서는 RDD는 변경할 수 없는(Immutable) 데이터의 집합으로 클러스터 내의 여러 노드에 분산되어 있다.
- low-level API인 Transformation, Action을 사용할 때
- 데이터가 미디어와 같이 비구조화 형태로 되어 있을 때
- 데이터를 함수형 프로그래밍으로 조작하고 싶을 때
- 데이터를 처리할 때, 칼럼 형식과 같은 스키마를 사용하지 않을 때

### Dataframe
- RDD와 같이 Dataframe도 변경 할 수 없는 데이터 집합이다. 
- 데이터가 RDB의 관계형 테이블처럼 칼럼이 존재한다. 
- 대용량 데이터를 좀 더 쉽게 처리 할 수 있다.
- 구조화 되어 있기 때문에 여러 데이터 엔지니어들이 쉽게 Spark에 접근 할 수 있다.
- Spark 2.0에서 Dataframe API는 Datasets API와 통합되었다.

### Datasets
- Spark 2.0부터 Dataset은 2가지 특징(strongly-typed API, uptyped API)을 가지게 되었다. 
- Dataframe은 제네릭(generic) 객체인 Dataset[Row]의 집합이며 Alias이다. (DataFrame = Dataset[Row])
- Static-typing & runtime type-safety
  - Spark SQL을 사용하면 실행 할 때까지 문법 에러를 알 수 없다. 반면에 Dataframe과 Dataset은 컴파일 시점에 에러를 잡을 수 있다. Dataset API는 람다 함수 및 JVM 타입 객체로 표현되기 때문에, 컴파일 시점에 타입을 검사할 수 있다. 
- High-level abstraction and custom view into structured and semi-structured data
- Ease-of-use of APIs with structure
  - 구조적 형태는 Spark에서 데이터를 다루는데 한계가 있을 수 도 있다. Dataset은 쉽게 이해 할 수 있고, 데이터를 다양하게 표현 할 수 있다.
  - agg, select, sum, avg, map, filter, groupby를 사용하여 데이터를 다루는 것은 RDD의 각 row 필드를 사용하는 것보다 쉽다. 

https://www.databricks.com/blog/2016/07/14/a-tale-of-three-apache-spark-apis-rdds-dataframes-and-datasets.html
