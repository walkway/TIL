# LinkedBlockingQueue

- thread-safe
- Queue가 꽉찼을 때 삽입 시도: blocking
- Queue가 비어있을 때 추출 시도: blocking
- blocking queue가 linked node로 연결
- 생산자가 많고, 하나의 소비자인 경우 사용
- poll(): 비어있는 경우 null
- take(): 꺼낼 수 있는 상태까지 wait
- queue가 비어있는 경우, 추출 시도 -> wait상태
- capacity설정하지 않는 경우, integer.MAX_VALUE로 설정

http://www.javarticles.com/2016/11/linkedblockingqueue-example.html