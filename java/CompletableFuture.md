# CompletableFuture

- 비동기
- Future, CompletionStage 상속
- CompletableFuture에 콜백을 등록해서 Future가 동작이 끝나고, 결과를 생산했을 때, 어떤 코드를 실행하도록 지정할 수 있다.
````
System.out.println("START");
CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(() -> {
    try {
        TimeUnit.SECONDS.sleep(2L);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return 200;
});
completableFuture.thenAccept(System.out::println);
System.out.println("END");
````
````
START
END
200
````
````
CompletableFuture.supplyAsync(this::findReceiver)
                     .thenApply(this::sendMsg)
                     .thenAccept(this::notify);
````

- delay
````
future.completeAsync(supplier, CompletableFuture.delayedExecutor(3, TimeUnit.SECONDS))
		.thenAccept(result -> System.out.println("accept: " + result));
````	

- Timeout
````
// TIMEOUT = 3;
// doWork() takes 5 seconds to finish
CompletableFuture<String> future = 
		doWork("JavaSampleApproach")
		.orTimeout(TIMEOUT, TimeUnit.SECONDS)
		.whenComplete((result, error) -> {
			if (error == null) {
				System.out.println("The result is: " + result);
			} else {
				System.out.println("Sorry, timeout in " + TIMEOUT + " seconds");
			}
	});
````
- xxxAsync: is executed using this stage's default asynchronous execution facility
- runAsync: Runnable 인터페이스 사용, ForkJoinPool에서 동작한다.
- supplyAsync: Supplier 인터페이스 사용, ForkJoinPool에서 동작한다.
- thenApply(T -> U): 결과에 함수를 적용한다.
- thenAccept(T -> void): thenApply와 비슷하지만 결과가 void다.
- thenCompose(T -> CompletableFuture<U>): 결과에 함수를 호출하고 반환받은 퓨처를 실행한다.
- handle((T, Throwable) -> U): 결과나 오류를 처리하고 새로운 결과를 돌려준다.
- whenComplete((T, Throwable) -> void): handle과 비슷하지만 결과가 void다.
- exceptionally(Throwable -> T): 오류를 기본 결과로 바꾼다.
- thenRun(Runnable): 결과가 void인 Runnable을 실행한다.
