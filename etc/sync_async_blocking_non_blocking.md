# Synchronous / Asynchronous
- 호출한 결과의 완료 확인 여부 / 호출자 입장 / 작업 완료 여부를 누가 신경쓰는지
- Synchronous 동기: 메소드를 호출하는 호출자(caller)가, 메소드에서 리턴(혹은 예외 발생)을 받기 전까지 다음 프로세스를 진행하지 못하고 기다리는 것
- Asynchronous 비동기: 호출자는 메소드 호출 후 다음 프로세스를 계속 진행, 이 때 메소드의 완료 및 리턴은 다른 방법(Callback, Future 또는 message 등)으로 받음, 

# Blocking / Non-blocking 
- 대기큐와 호출 결과 시점으로 구분 / 제어권 관점
- Blocking: 하나의 쓰레드가 다른 쓰레드를 무한정 기다리게 만드는 것 / 호출된 함수가 자신의 작업을 모두 마칠 때까지, 호출한 함수에게 제어권을 넘겨주지 않고 대기
- Non-blocking: 어떤 쓰레드도 다른 쓰레드를 무한정 기다리게 하지 않는 것 / 호출된 함수가 바로 return해서 호출한 함수에게 제어권을 넘겨주고. 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있음
  - 블로킹 연산이 포함되어 있을 경우, 시스템의 전체적인 진행을 보장할 수 없음

## Non-blocking & Synchronous

## Blocking & Asynchronous
- Asynchronous > A는 B의 return에 신경쓰지 않고, 콜백 함수를 넘김
- Blocking > A는 B 작업에 관심 없지만, B에게 제어권을 넘김 / A는 자신과 관련 없는 B의 작업이 끝날 때까지 기다림

## Non-blocking & Synchronous
- A -> B 호출
- Non-blocking > A는 B에게 제어권을 주지 않고, 계속 진행
- Synchronous > A는 B의 return이 필요하기 때문에, 중간마다 B에게 완료 확인
## Non-blocking & Asynchronous
- A -> B 호출
- Non-blocking > A는 B에게 제어권을 주지 않고, 계속 진행 / B 를 호출한 이후에도 멈추지 않음
- Asynchronous > A는 B를 호출할 때 콜백 함수를 넘김. B는 자신의 작업이 끝나면 A가 준 콜백 함수를 실행

https://medium.com/@ahaljh/%EB%8F%99%EC%8B%9C%EC%84%B1-%EA%B4%80%EB%A0%A8-%EA%B0%9C%EB%85%90-d2f3e6a62b99
https://luminousmen.com/post/asynchronous-programming-blocking-and-non-blocking