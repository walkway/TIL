# ThreadPool

- 병렬 작업의 증가로 인한 스레드의 폭증을 막기 위해서 사용한다.
- 스레드를 제한된 개수만큼 정해둔다.
- 각 스레드는 작업 큐에 들어오는 작업을 처리한다.
- ExecutorService
````
ExecutorService executorService = Executors.newCachedThreadPool();
````
- 종료
````
executorService.shutdown();
````
- 강제종료
````
executorService.shutdownNow();
````
- Runnable: return 값 없음
- Callable: return 값 있음
- Result객체: 공유객체, 두개 이상 스레드 작업 취합
- ExecutorService submit(): 작업을 스레드 풀의 큐에 저장하고, Future 객체 리턴
- Future get(): 스레드가 작업을 완료할 때까지 블로킹, 작업을 완료하면 처리 결과를 리턴