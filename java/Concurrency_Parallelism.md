# Concurrency_Parallelism

## Concurrency
- 동시에 실행되는 것 같이 보이는 것	
- 여러 계산이 동시에 실행되는 컴퓨팅 형태
- 논리적인 개념
- 싱글 코어 > context switch, thread 간 빠르게 교차되며 실행되어서 동시라고 느끼는 것
  - 동시 작업이 너무 많다면 문맥 교환의 오버헤드로 인해 싱글 코어에서 싱글 스레드로 작업하는 것이 더 빠를 수 있음
- concurrently(O) / simultaneously (X)

## Parallelism
- 실제로 동시에 여러 작업이 처리되는 것
- 많은 계산 또는 프로세스 실행이 동시에 수행되는 계산 유형
- 물리적인 개념
- concurrently(O) / simultaneously (O)

### Race Condition
- 경쟁 상태, 둘 이상 조작이 결과에 영향을 줄 수 있음
// 작성

### Critical Section
- 병렬 컴퓨팅에서 둘 이상의 스레드가 동시에 접근해서는 안되는 공유 자원(자료 구조 또는 장치)을 접근하는 코드의 일부
- 어떤 스레드가 임계 구역에 들어가고자 한다면, 지정된 시간만큼 대기

// 작성