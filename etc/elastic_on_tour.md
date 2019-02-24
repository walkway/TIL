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