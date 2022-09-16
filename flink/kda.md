# KDA
- Amazon Managed Service for Apache Flink를 사용하면 Java, Scala 또는 SQL을 사용하여 스트리밍 데이터를 처리하고 분석할 수 있다.
- 스트리밍 소스에 대해 코드를 작성 및 실행하여 시계열 분석을 수행하고, 실시간 대시보드를 제공하고, 실시간 메트릭을 생성할 수 있다.
- Apache Flink 기반 오픈 소스 라이브러리를 사용하여 Kinesis Data Analytics에서 Java 및 Scala 애플리케이션을 구축할 수 있다.
- Apache Flink는 데이터 스트림 처리에 널리 사용되는 프레임워크 및 엔진이다.
- Kinesis Data Analytics는 Apache Flink 애플리케이션을 위한 기본 인프라를 제공한다.
- 컴퓨팅 리소스 프로비저닝, 병렬 계산, 자동 확장 및 애플리케이션 백업(체크포인트 및 스냅샷으로 구현)과 같은 핵심 기능을 처리한다.
- Flink 인프라를 직접 호스팅할 때 사용하는 것과 동일한 방식으로 고급 Flink 프로그래밍 기능(예: 연산자, 함수, 소스 및 싱크)을 사용할 수 있다.
- Kinesis Data Analytics는 대부분의 속성에 대해 Apache Flink 권장 값과 공통 애플리케이션 프로필을 기반으로 하는 몇 가지 값으로 구성된 기본 Flink 구성을 제공하ㄴ다.  
- 서비스 제공 기본 구성은 대부분의 애플리케이션에서 작동한다.
- 병렬 처리, 메모리 및 상태 사용량이 많은 특정 응용 프로그램의 성능을 개선하기 위해 Flink 구성 속성을 조정하거나 Apache Flink에서 새로운 디버깅 기능을 활성화해야 하는 경우 지원 사례를 요청하여 특정 속성을 변경할 수 있다.
- Kinesis Data Analytics는 상태 백엔드에 임시 데이터를 저장한다. Kinesis Data Analytics는 RocksDBStateBackend 를 사용한다. 다른 백엔드를 설정하기 위해 호출 setStateBackend해도 영향이 없다.

https://docs.aws.amazon.com/kinesisanalytics/latest/java/reference-flink-settings.title.html
https://docs.aws.amazon.com/kinesisanalytics/latest/java/what-is.html
