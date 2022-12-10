# AWS re:Invent

### Zero-ETL
- Amazon Aurora zero-ETL integration with Amazon Redshift
- Amazon Aurora는 Amazon Redshift와 Zero ETL 통합 지원을 통해 Aurora의 페타바이트 규모 트랜잭션 데이터를 Amazon Redshift를 사용하여 실시간에 가까운 분석 및 기계 학습(ML)을 활성화한다. 트랜잭션 데이터가 Aurora에 기록된 후 몇 초 이내에 Amazon Redshift에서 데이터를 사용할 수 있고, 따라서 추출, 전환, 적재(ETL) 작업을 수행하기 위해 복잡한 데이터 파이프라인을 구축 및 유지할 필요가 없다.
  - https://aws.amazon.com/ko/about-aws/whats-new/2022/11/amazon-aurora-zero-etl-integration-redshift/
- Amazon Redshift auto-copy from S3: S3 데이터를 손쉽게 Redishift로 옮기도록 자동화

- 새로운 기능은 고객이 AWS에서 제로 ETL의 미래를 향해 나아갈 수 있도록 서비스 간에 수동으로 데이터를 이동하거나 변환할 필요성을 줄여준다.
- 고객은 아마존 레드시프트(Amazon Redshift)를 사용해 거의 실시간으로 아마존 오로라(Amazon Aurora) 데이터를 분석할 수 있게 되어 서비스 간에 데이터를 추출, 변환, 로드(ETL)할 필요가 없다. 또한 고객은 이제 아마존 EMR, AWS 글루(AWS Glue), 아마존 세이지메이커(Amazon SageMaker) 등 AWS 분석 및 머신러닝(ML) 서비스를 사용해 아마존 레드시프트 데이터에서 아파치 스파크(Apache Spark) 애플리케이션을 쉽게 실행할 수 있다. 
- 고객을 위해 ETL과 기타 데이터 이동 작업을 제거함으로써, AWS는 고객이 조직과 데이터의 규모와 복잡성에 관계없이 데이터 분석과 비즈니스에 대한 새로운 인사이트를 도출하는 데 집중할 수 있도록 지원한다.
- 아마존 레드시프트와 아마존 오로라의 제로 ETL 통합을 통해, 트랜잭션 데이터는 아마존 오로라에 기록되고 아마존 레드시프트에서 원활하게 사용할 수 있게 된 후 몇 초 후에 자동으로 지속적으로 복제된다. 아마존 레드시프트에서 데이터를 사용할 수 있게 되면, 고객은 데이터 분석을 즉시 시작하고 데이터 공유나 아마존 레드시프트 ML과 같은 고급 기능을 적용하여 총체적이고 예측 가능한 인사이트를 얻을 수 있다. 
- 아파치 스파크를 위한 아마존 레드시프트 통합으로 개발자는 AWS 지원 분석 및 ML 서비스를 사용해 아파치 스파크 애플리케이션을 아마존 레드시프트의 데이터에서 보다 쉽게 구축하고 실행할 수 있다. 아파치 스파크를 위한 아마존 레드시프트 통합은 AWS에서 인증, 패키징 및 지원되므로 서드파티 커넥터와 관련된 번거롭고 오류가 발생하기 쉬운 프로세스가 제외된다. 
- 개발자는 주요 언어 프레임워크(자바, 파이썬, R, 스칼라 등)를 사용해 몇 초 내에 아파치 스파크 기반 애플리케이션에서 아마존 레드시프트 데이터에 대한 쿼리 실행을 시작할 수 있다. 중간 데이터 스테이징 위치는 자동으로 관리되므로 고객이 애플리케이션 코드에서 이를 구성하고 관리할 필요가 없다. 

https://www.youtube.com/watch?v=xMnRzeHDRZU
https://www.itworld.co.kr/news/268173#csidx9c2a1efe10a1817a5512a4ee1c6e998 
