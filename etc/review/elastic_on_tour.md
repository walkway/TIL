# Elastic On Tour Opening
- scale, relevance, speed
  - 3가지 균형을 통해, 성공적인 유저 경험을 만든다.
- 단순한 search box에서 histogram
  - 시간의 흐름에 따라 변하는 데이터
  - 시간 기반, 로깅, 쿼리를 했을 때, 탑 10의 데이터 -> 데이터 변화에 따라 매칭하는?
- 유저들이 로그를 수집해서 metric에 넣는 것을 알고 -> apm 데이터 발견 / 결합해서..-..-?
- kibana: 시각화 대시보드 경험을 만든다.
- filebeat, logstash: 데이터의 이동을 쉽게
- 제품은 자주 빠르게 배포하고, 지속적인 투자를 한다.
- 한국어 구문 분석기 nori 출시
- x-pack(상용 기능 플러그인) 코드 오픈
 - elastic: paid(code open)/free

-----

# What’s Evolving in the Elastic Stack
- elastic stack 진화: stack -> cloud -> solution
- 내가 가진 데이터 -> 엘라스틱 서치 -> 수집 -> 시각화
- 목적에 맞는 모양으로 구성해주는 솔루션 제공
- 데이터 수집 시 표준화하는 모듈 제공?
- Elastic Common Schema
  - 데이터 분석하다보니 공통 필드들이 보여서 필드 스키마 표준화
  - https://github.com/elastic/ecs
- Auditbeat: 파일 통합 모니터링 및 리눅스 커널 검사
  - Linux, macOS, Windows의 파일 변경 감시
  - 짧은 시간동안 실행된 프로세스 및 연 감지
  - Elasticsearch로 직접 색인
  - 커널 감사 이벤트와 상호 작용
  - 사용자 ID로 부터 사용자 이름 파악
- 컨테이너 아키텍처에서는 모든 표적들이 움직인다..?
  - 클라우드 -> 컨테이너
  - docker, kubernetes에서도 데이터를 수집할 수 있어야 한다.
- 수집 회복성 resilience
  - logstash: persistent queue
  - input -> (diskqueue) -> filter -> output / pipeline
  - 중간에 다운되면 diskqueue에 있는 데이터로 복원, 데이터 유실 x
  - 정합성: 제대로 색인이 안되면..? dead letter queue
  - end to end acking: 긴 데이터 여정에서 유실이 없도록.
- 검색
  - query ui로 선택
  - KQL
  - cross cluster search
    - 서로 다른 메이저간 클러스터 검색
- Seure All the Things
  - 노드들 간 통신은 의무적으로 TLS 활성
  - 기본 패스워드 없음(changeme): 쉽게 설정하라고 추가했지만, 오히려 해킹 위협
- Kibana space
  - admin, guest
  - space 별 권한
- 관리 administration
  - 메이저 버전 간 롤링 업데이트
  - 파일 기반 복구: 파일에서 다른 것 비교해서 복구 -> 시간이 소요됨
  - Opt-Based Recovery(6.0): 복구 이후, 명령 전달해서, 명령어를 다시 수행하도록 변경. 빠르고 안정적으로 복구되는 방법
- 고정(Frozen) 인덱스
  - 검색 속도를 내어주고 더 큰 디스크 저장공간을 얻는다.
  - 오래된 데이터를 저장하고 검색하기 위한 목적으로 사용
  - 매우 적은양의 힙 사용
  - 순서대로 인덱스를 오픈하고 검색
  - 리플리카 생성 - 데이트 유실 방지
- index lifecyle management
  - hot nodes - warm nodes - cold nodes

-----

# Canvas for Building Real-Time Infographics
- http://canvas.elastic.co
- 의미 있는 숫자들이 의미있게 보이도록 도와준다.
- infographics
- reports
- presentations
- dashboards
- digital displays & kiosks
- 라이브 데이터, 드래그 앤 드롭, 폰트/색상, 페이지 레이아웃, export
- 디스플레이 타입 변경 가능
- 데이터 -> 쿼리 -> 화면 표시
- 코드 기반으로 asset을 가져온다.

-----

# End-to-End Security Analytics with the Elastic Stack
- collect -> normalize -> enrich -> index
  - collect: all parts of the puzzle
  - normalize for aggregation and correlation accross sources
  - enrich to extend attribute
- Anomaly-based Detection
  - using elastic machine learning
  - 무엇이 정상인가?
  - 갑자기 비상정적인 행동
  - 정상적인 로그인 행동이 아닌
  - 다른 유저들과 다른 행동

-----

# Nori: The Official Elasticsearch Plugin for Korean Language Analysis
- 오픈소스 한국어 분석기는 있고, 공식은 없었다.
  - seunjeon, ariang, open korean text
- 어떤 한국어 분석기를 사용할까 https://www.elastic.co/kr/blog/using-korean-analyzers
- find a way to properly support korean!
- nori(놀이): 공식 한국어 분석 플러그인

### Nori, the genesis
- MeCab
  - open source text segmentation library
  - 일본어 사전
  - Lucene Kuromoji Analyzer
- mecab-ko-dic
 - 한국어를 형태학적으로 분석하는 모델을 정의
 - created by Yongwoon Lee and Yungho Yu
 - apache 2 license
 - 형태소 사전(morphological dictionary) for Korean language using MeCab
 - More than 800,000 entries
  - 3815 left ids, 2690 right ids(connection costs 3815*2690)
 - used by seunjeon
 - 200mb uncompressed

### Binary Dictionary
- finite state transducer(FST)
- 한글, 한자 Prefix and infix compression
- UTF-16 encoding
- 5.4MB
- connction costs
  - 3815 left ids, 2690 right ids
  - one short(16bits)per cell
  - 20mb loaded in a direct byte buffer outside of the heap
- feature encoding
  - custom binary encoding
  - 9 bytes per entry (7mb total)
### 형태소 분석(morphological analysis)
- viterbi algorithm
- lattice
  - find the best possible segmentation
  - backtrace when only one path is alive
- example
  - 21세기 세종계획
  - 21 + 세기(century) + 세종(sejong) + 계획(plan)
### custom dictionry
- 도메인 특성적인 단어 수가 많으면, 추가 규칙을 사용하여 원본 사전을 다시 작성할 수 있다.
- lower the memory usage compared to the user-dic approach
- analyzer/tokenizer 생성 속도 향상
- 분석 속도 향상
- https://github.com/jimczi/nori/blob/master/how-to-custom-dict.asciidoc

### additional filters
- nori_part_of_speech token filter
  - 품사 태그 제거
  - lucene.apache.org/core/7_7_0/analyzers-nori/org/apache/lucene/analysis/ko/POS.Tag.html
- nori_readingform
   - 한자에서 한글

### Nori, future
- keep N best segmentations
- upgrade to the latest mecab-ko-dic
- find longest token in the user dictionary

# Logging, Metrics, and APM: The Operations Trifecta
- apm: 애플리케이션 성능 관리 (Application Performance Management)
- metrics: KPI 주기적인 측정/ every x minutes, measure the CPU load and print it out, and annotate with meta-data
- log: event 발생 기록/ for each event, print out what happend, 메타 데이터, 텍스트 중심
- 엘라스틱 스택을 사용한다면 단일화된 대시보드 확인 가능
  - 단일화된 alert, 전략
  - unified machine learning
- rollups: long-term retention
  - roll up or aggregate older data into bigger time buckets and save on disk space
  - 현재는 몇 초 데이터를 원하지만, 한달 뒤 1분단위로 필요하다면?
- elastic stack for apm
  - 일반 로그 리퀘스트가 서버로 들어오고, 500 응답을 주면, 왜 응답을 주었는지 이해하기 어렵다.
  - 리퀘스트가 7초가 걸렸다. 왜 느려졌는지 알 수 없다.
  - apm adds end-user experience and application-level monitoring to the stack
  - apm is another index in elasticsearch
  - 자세한 성능 정보를 수집하여 소프트웨어 서비스 및 응용 프로그램을 실시간으로 모니터링 할 수 있으므로 성능 문제를 신속하게 찾아 내고 수정할 수 있다.
  - agent -> apm-server-> elasticsearch -> kibana

# Machine Learning Deep Dive
- 정상적인 모델, 이상적인 모델
- 비지도 학습
- 시계열 데이터 활용: 대부분 엘라스틱 고객들이 사용
- 이상징후를 어떻게 도출하는지
- product 철학: operatinalize data science for everyone
- learn - predict - operationalize
- rules 추가 ex) 숫자 범위 지정, 특정 날짜 제외-달력
- 머신 러닝 기능을 나의 상황 + 업무에 맞게 커스텀을 할 수 있음.

-----

# Order monitoring and anomaly detection with Elastic Stack
- 2017년 마이크로서비스 아키텍처 전환
- 트래픽이 늘어나니, orcle에서 문제 발생
- 주문과 결제 부분의 문제를 해결하기 위해서 Kafka를 이용한 이벤트 기반의 아키텍쳐 도입

### request driven model vs event driven model
- request driven model
  - 결합도: request, response 가 서로의 존재를 알고 있다.
  - 요청의 결과: 응답이 오거나, 실패 한 원인을 알 수 있다.
  - 처리 시간: 수 초 이내
- event driven model
  - 결합도: 누가 처리를 하는지, 누가 호출을 했는지 관심사 밖
  - 요청의 결과: 처리의 결과를 모른다. 누가 처리 했는지 모른다.
  - 처리 시간: 수 초 이내 or 언젠가는..

### 아키텍처의 변화에 따른 모니터링의 변화, 데이터 조회
- avro로 래핑된 데이터를 어떻게 볼 것인가
- event의 생성, 유실, 중복 처리 등은 어떻게 확인 할 것인가
- 잘못된 주문, 결제 지연 등은 어떻게 감지 할 것인가
- making application
  - (+) 원하는 대로 만들 수 있다.
  - (-) 누가 만들지, 장애 감지 목적인데.. 누가 또 감지할지, 주문 결제 개선? 모니터링 개발? 우리의 목적은?
- using elastic stack
  - (+) 쉽고 빠른 결과, 기본적인 ha 지원
  - (-) 모든 것을 다 할 수 없다. watcher, vega, visual builder 등 툴 학습
- 엘라스틱 스택 사용 결정, kafka -> logstash -> elasticsearch

### 데이터의 수집과 표현
- 주문 데이터의 수집 logstash with avro codec plugin
- today dashboard
  - 결제 금액, 결재 실패율, 1분당 비동기 주문 요청, 결제 수단별 결제 요청 결과, 시간별 결제 요청 방법, 결제 실패시 응답 메시지, 제품 판매 순위
  - 한 화면을 보면서 얘기를 할 수 있는 것이 생김
- 개발자를 위한 message flow dashboard
  - 파티션 분포도
  - 초당 이벤트 갯수 요청 실패

### 장애 감지와 통보
- watcher & slack notification
  - rollback_complted 되지 않은 실패 건이 발생할 경우
  - 주문이 결제 처리량보다 많을 경우(주문이 즉시 결제되지 않을 경우): ex 최근 5분 pas 호출 지연이 1초 이상 352개 입니다. 파티션이나 permit체크!
  - 결제 실패(성공) 메시지가 유실된 경우

### 머신 러닝을 통한 개선
- 악의적인 대량 주문 감지
 - ex 면도기가 갑자기 100대 이상 팔리거나
 - 카테고리마다 판매되는 개수 금액이 달라서, 동일한 컨디션으로 판단할 수 없음
 - 완벽하지 않지만 좀 더 많은 가치를 줄 수 있음
- 새로운 발견
 - ex 새벽에 고액의 결제 요청을 하지만 실패, 특정 고객이 1억원 이상을 일주일에 걸쳐서 계속 시도
 - 이 사람이 진짜 1억원을 결제하려고한건아닐까? -> 그럼 고객센터 연결해서 도움을 주어야하나?
 - 시스템에서 1억원 결제 되는가?
 -> 새로운 인사이트를 알게된다.

### 정리
- elk는 도구이다. 도구를 잘 활용해서 우리의 진짜 목적을 달성한다.
- 도구는 쉬워야하는데 쉽다.
- 계속된 기능의 확장, 다양한 활용 범위

-----

# 채널시스템의 엘라스틱 활용사례
- 디리아: 다양해지는 각 채널 별로 상이한 통신방식과 데이터 형태를 표준화하여 복잡한 거래를 빠르고 안정적이며 정확하게 처리
- 기존의 채널 로그 활용: 축적된 많은 데이터 가치있게 -> elastic stack
- 카드 van 승인 채널 사례
  - 전체 승인/성공/오류 건수/ 이상거래 건수
  - 시계열 차트 오류 건수/응답시간/타임아웃/오류유형
  - 가맹점 정보: 거래 위치, 오류건수, 탑5 가맹점
  - 오류 정보 상세, ap처리 로그
- 특정 상황에 대하여 해당 거래 쉽게 추적 검색
  - 동일 데이터로 전체화면 연동
  - drill-down으로 직관적 검색
- 로그 기반 검색 조건 제공
  - 조건: 카드사, 카드번호, 시간, 금액
  - 카드사 정보: 카드사별 거래비율, 실패 건수, 응답시간, 에러 상세 내역
- 거래로그 이용하여 부정사용 의심거래 탐지
 - 예: 동일카드로 이전 거래 시간대비 실제 가맹점간 거리 계산 후 임게치 비교
 - 탐지하는 경우 슬랙을 통해서 메시지 전달
- 부정사용 의심거래 탐지
 - 전/후 시간차, 가맹점 간 거리, 거래 정보