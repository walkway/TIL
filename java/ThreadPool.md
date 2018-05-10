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