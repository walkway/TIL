# Configuration

## Advanced Options for Flink Web UI
- web.refresh-interval | 3000	| Long	
  - Refresh interval for the web-frontend in milliseconds.
- web.checkpoints.history | 10 | Integer
  - Number of checkpoints to remember for recent history. 

## History Server
- 해당 Flink 클러스터가 종료된 후 완료된 작업의 통계를 쿼리하는 데 사용할 수 있는 히스토리 서버
- HTTP 요청을 수락하고 JSON 데이터로 응답하는 REST API를 노출
- HistoryServer를 사용하면 JobManager에 의해 보관된 완료된 작업의 상태 및 통계를 쿼리할 수 있음
- HistoryServer 및 JobManager 를 구성한 후 해당 시작 스크립트를 통해 HistoryServer를 시작 및 중지
- 기본적으로 8082 port 사용
- 독립 실행형 프로세스로만 실행
````
# Start or stop the HistoryServer
bin/historyserver.sh (start|start-foreground|stop)
````
