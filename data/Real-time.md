# Real-time
- Oracle CEP, BusinessEvents, IBM InfoSphere Streams, 오픈소스 Esper 
- 실시간 = Real-time, Real Real-time
 - Batch : 초,분,시간 수준의 일괄처리 
 - Near Real-time : 초단위 수준의 지연시간 보장 
 - Real-time : Milliseconds 수준의 처리 보장 
 - Real Real-time : Microseconds 수준의 처리 보장
### CEP
- Complex Event Processing
- DB나 File, Hadoop에 저장하지 않고, 다양한 고속의 이벤트 스트림을 In-Memory 기반으로 초당 수 백/수십 만건의 Real-time 처리
- 선분석처리, 후저장 방식
- 데이터 용량에 상관없이 Low-Latency 보장
http://www.dbguide.net/knowledge.db?cmd=specialist_view&boardUid=180895&boardConfigUid=108&boardStep=0&categoryUid=