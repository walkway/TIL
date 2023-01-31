# Amazon DynamoDB
- 모든 규모에서 고성능 애플리케이션을 실행하도록 설계된 완전 관리형의 서버리스 키-값  NoSQL 데이터베이스
- 보안, 지속적인 백업, 자동화된 다중 리전 복제, 인 메모리 캐시 및 내보내기 도구를 제공
- 키 값과 문서 데이터 모델을 지원
- 작은 규모에서 시작했다가 전역으로 확장하여 초당 페타바이트 단위의 데이터와 수천만 건의 읽기 및 쓰기 요청을 지원
- 고성능 애플리케이션을 실행할 수 있도록 설계
- Accelerator를 사용하여 지연 시간을 마이크로 초 수준으로 최소화D
- 온디맨드와 프로비저닝의 용량 모드를 제공하여 비용 절약 가능
- ACID(Atomicity, Consistency, Isolation, Durablility) 트랜잭션 지원
- 저장 중 암호화 지원
- 특정 시점으로 복구 가능(Point-In-Time Recovery)
- NoSQL Workbench: Data modeling, Data visualization, Operation building
  - Amazon DynamoDB용 NoSQL Workbench는 최신 데이터베이스 개발 및 운영에 사용할 수 있는 플랫폼 간 클라이언트 측 GUI 애플리케이션
  - Windows, macOS, Linux에서 사용 가능
  - https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html

## 파티션
- Amazon DynamoDB는 데이터를 파티션에 저장
- 파티션 관리는 DynamoDB에서 전적으로 처리하므로 사용자가 파티션을 직접 관리할 필요가 없음
- 테이블이 단순 기본 키를 가질 경우(파티션 키만 있음) DynamoDB가 파티션 키 값을 기준으로 각 항목을 저장하고 검색
- 테이블에 항목을 쓰기 위해 DynamoDB는 내부 해시 함수에 대한 입력으로 파티션 키 값을 사용, 해시 함수 출력 값은 항목을 저장할 파티션을 결정
- DynamoDB는 파티션 수와 상관없이 항목을 테이블의 파티션에 균일하게 분배하는 데 최적화되어 있음. 테이블 항목 수에 비해 많은 수의 고유 값을 가질 수 있는 파티션 키를 선택할 것 권장
https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/HowItWorks.Partitions.html

## 온디맨드
- 애플리케이션이 테이블에서 수행하는 데이터 읽기 및 쓰기 요청에 대해 요금이 청구
- DynamoDB가 워크로드의 확장 또는 축소에 따라 워크로드를 즉시 조정하므로 사용자가 애플리케이션에 대해 예상하는 읽기 및 쓰기 처리량을 지정할 필요가 없음
- 온디맨드 모드는 다음 중 하나에 해당되는 경우에 유용한 옵션
  - 알 수 없는 워크로드를 포함하는 테이블을 새로 만들 경우
  - 애플리케이션 트래픽이 예측 불가능한 경우
  - 사용한 만큼에 대해서만 지불하는 요금제를 사용하려는 경우

## 프로비저닝
- 애플리케이션에 필요한 초당 읽기 및 쓰기 횟수를 지정
- Auto Scaling을 사용하여 트래픽 변경에 따라 테이블의 프로비저닝된 용량을 자동으로 조정
- 프로비저닝된 모드는 다음 중 하나에 해당되는 경우에 유용한 옵션
  - 애플리케이션 트래픽이 예측 가능한 경우
  - 트래픽이 일관되거나 점진적으로 변화하는 애플리케이션을 실행할 경우
  - 비용 관리를 위해 용량 요구 사항을 예측할 수 있는 경우

## 읽기 요청 유닛
- 테이블에서 데이터를 읽는 API 호출은 읽기 요청 유닛 단위로 요금이 청구
- 최대 4KB 기준, 4KB보다 큰 경우 추가 읽기 요청 유닛을 필요
- 강력한 일관된 읽기 요청: 1개의 읽기 요청 유닛을 필요
- 최종적 일관된 읽기 요청: 0.5개의 읽기 요청 유닛을 필요
- 트랜잭션 읽기 요청: 2개의 읽기 요청 유닛을 필요

## 쓰기 요청 유닛
- 테이블에 데이터를 쓰는 API 호출은 쓰기 요청 유닛 단위로 요금이 청구
- 최대 1KB 기준, 1KB보다 큰 경우 추가 쓰기 요청 유닛을 필요
- 트랜잭션 쓰기 요청: 2개의 쓰기 요청 유닛을 필요
  - 예를 들어 1KB 항목의 쓰기 요청은 1개의 쓰기 요청 유닛을 필요로 하고, 3KB 항목의 쓰기 요청은 3개의 쓰기 요청 유닛을 필요로 하며, 3KB 항목의 트랜잭션 쓰기 요청은 6개의 쓰기 요청 유닛을 필요로 함

# AWS Glue
- AWS Glue는 분석, 기계 학습 및 애플리케이션 개발을 위해 데이터를 쉽게 탐색, 준비, 그리고 조합할 수 있도록 지원하는 서버리스 데이터 통합 서비스
- 사용자는 AWS Glue 데이터 카탈로그를 사용하여 데이터를 쉽게 찾고 액세스할 수 있음
- 데이터 엔지니어와 ETL (추출, 변형 및 로드) 개발자는 AWS Glue Studio에서 ETL 워크플로를 시각적으로 생성, 실행 및 모니터링할 수 있음
- 소유한 데이터의 속성을 검색하고, 데이터를 변환하여 분석용으로 준비하는 데 사용
- Glue는 Amazon S3의 데이터 레이크, Amazon Redshift의 데이터 웨어하우스, AWS에서 실행되는 다양한 데이터베이스에 저장된 정형 및 반정형 데이터를 모두 자동으로 검색 가능
- ETL에서 사용할 수 있고 Amazon Athena, Amazon EMR 및 Amazon Redshift Spectrum과 같은 서비스에서 쿼리 및 보고하는 데 사용할 수 있는 Glue 데이터 카탈로그를 통해 데이터에 대한 통합된 뷰를 제공
- ETL 작업을 위한 Scala 또는 Python 코드를 자동으로 생성

### ETL 오류 처리
- AWS Glue는 작업 이벤트 지표와 오류를 모니터링하고 모든 알림을 Amazon CloudWatch로 푸시
- Amazon CloudWatch에서는 AWS Glue의 특정 알림에 따라 트리거되는 호스트 작업을 구성할 수 있음, 예를 들어 Glue로부터 오류 또는 성공 알림을 수신하면, AWS Lambda 함수가 트리거되도록 할 수 있음
- 기본 재시도 동작을 제공하므로 오류 알림을 전송하기 전에 모든 실패 작업을 3번 재시도

# AWS Step Functions
- 각 단계가 이전 단계의 출력을 입력으로 수신하는 여러 단계로 구성된 워크플로를 설계 및 실행하여 조정 작업을 보다 쉽게 도와주는 완전관리형 서비스
- 유연한 병렬/분기의 흐름을 제어, 워크플로우 시각적으로 확인
- 워크플로우 형태로 가시화하는 에디터 기능 제공
- AWS 서비스들을 조직화하여 단계별로 실행
- 각 단계를 자동으로 트리거하고 추적
- State (상태)를 사용한 분산 트랜잭션
- Lambda Function들을 유기적으로 연결할 수 있음
  - Lambda: 메모리 할당 128MB ~ 10,240MB 까지 설정 가능, 900초의 제한 시간, 스토리지 용량 512MB 고정
- 각 Step Functions 상태 머신에는 여러 개의 동시 실행이 있을 수 있음
  - Step Functions 콘솔에서 시작하거나, AWS SDK, Step Functions API 작업 또는 AWS Command Line Interface (AWS CLI) 를 사용하여 시작할 수 있음
- https://docs.aws.amazon.com/ko_kr/step-functions/latest/dg/tutorial-get-started-parallel-tasks.html
- https://labs.brandi.co.kr//2022/01/04/kimsj7.html