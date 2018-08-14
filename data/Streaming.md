# Streaming

### 한정 데이터(Bounded data)
- 이미 저장된 데이터를 처리하는 것, 별도의 처리 패턴 없음

### 무제한 데이터(Unbounded data)
- Batch: 스트리밍 데이터를 일정 시간 단위로 모아 처리하는 방식. 구현이 간단, 데이터 수집 후 처리 -> 실시간 처리 x
- Streaming: Time agnostic, Filtering, Inner join, Windowing 방식, 배치에 비해 복잡, Skew가 환경에 따른 변화가 심함 -> 데이터가 시스템에 도착하는 순서 순차적이지 않음.
 - Time agnostic : 시간 속성이 없는 데이터로 들어오는 순서대로 처리
 - Filtering : 들어오는 데이터 중 특정 데이터만 필터링 후, 저장
 - Inner join : 교집합이라는 말처럼, 두개의 무제한 데이터에서 들어오는 값을 비교, 매칭시켜 값을 구별
 - Windowing : 스트리밍 데이터를 일정한 시간 간격으로 처리