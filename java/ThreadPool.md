# ThreadPool

- 스레드가 몇 개 만들어질지 정해지지 않은 프로그램은 너무 많은 스레드가 한번에 실행될 가능성이 있다.
- 그 결과, 동작 중인 컴퓨터의 메모리 자원을 다 써버려서 처리를 계속할 수 없게 된다.
- 스레드 풀은 사용할 스레드들 제한된 수만큼 만들어두고 일정한 규칙에 따라 실행하는 기능이다.
- 병렬 작업의 증가로 인한 스레드의 폭증을 막기 위해서 사용한다.
- 스레드를 제한된 개수만큼 정해둔다.
- 각 스레드는 작업 큐에 들어오는 작업을 처리한다.
- 스레드는 execute 메소드로 시작하고, shutdown 메소드를 종료힌다.
- 종료는 실행 중인 스레드가 끝나야 종료 상태가 된다.
- ExecutorService
````
ExecutorService executorService = Executors.newCachedThreadPool();
````
- 종료: 남은 작업 마무리, 새로운 작업 수용하지 않음
````
executorService.shutdown();
````
- 강제종료: 남은 작업 상관없이 마무리, 실행 대기중인 작업 목록 리턴
````
executorService.shutdownNow();
````
- 타임아웃 종료: 모든 스레드가 종료될 때까지 대기 상태로 두었다가, 지정한 시간이 경과하면 종료
````
executorService.awaitTermination(5, TimeUnit.MINUTES)
````
````
// 타임아웃되면 메소드가 false 반환
if(!executorService.awaitTermination(5, TimeUnit.MINUTES)){
	executorService.shutdownNow();
}
````
- 셧다운 실행 후, 작업 종료 여부 확인
````
executorService.isTerminated();
````

- Runnable: return 값 없음
- Callable: return 값 있음
- Result객체: 공유객체, 두개 이상 스레드 작업 취합
- ExecutorService submit(): 작업을 스레드 풀의 큐에 저장하고, Future 객체 리턴
- Future get(): 스레드가 작업을 완료할 때까지 블로킹, 작업을 완료하면 처리 결과를 리턴
- Future get(long timeout, TimeUnit unit): timeout 시간 전 작업이 완료되지 않으면, TimeoutException

````
public class ResultByRunnableExam {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(
            Runtime.getRuntime().availableProcessors()
        );
        
        System.out.println("작업 처리 요청");
        
        class Task implements Runnable {
            Result result;
                
            Task(Result result) {
                this.result = result;
            }
            
            
            @Override
            public void run() {
                int sum = 0;
                
                for (int i = 1; i <= 10; i++) {
                    sum += i;
                }
                
                result.addValue(sum);
            }
        };
        
        Result result = new Result();
        Runnable task1 = new Task(result);
        Runnable task2 = new Task(result);
        Future<Result> future1 = executorService.submit(task1, result);
        Future<Result> future2 = executorService.submit(task2, result);
        
        try {
            result = future1.get();
            result = future2.get();
            System.out.println("처리 결과: " + result.accumValue);
            System.out.println("처리 완료");
        } catch (Exception e) {
            e.printStackTrace();
        }
        
        executorService.shutdown();
    }
}
 
class Result {
    int accumValue;
    synchronized void addValue(int value) {
        accumValue += value;
    }
}
````