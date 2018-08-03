# CompletableFuture

- 비동기
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

