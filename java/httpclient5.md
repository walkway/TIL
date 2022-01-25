# HttpClient 5

- Http/2.0 지원, 기반 기술이 완전한 비동기 구현체로 변경되어 비동기 응답을 처리할 수 있다.
````
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents.client5/httpclient5 -->
<dependency>
    <groupId>org.apache.httpcomponents.client5</groupId>
    <artifactId>httpclient5</artifactId>
    <version>5.1.2</version>
</dependency>
````
````
CloseableHttpAsyncClient client = HttpAsyncClients.createDefault();  
client.start();

SimpleHttpRequest httpGet =  
    SimpleRequestBuilder
    .get("https://naver.com")
    .build();
Future<SimpleHttpResponse> future = client.execute(httpGet, null);

assertEquals(200, future.get().getCode());

client.close();  
````
- 응답을 Future로 받는 Future 기반 방식을 사용하면 future.get() 메서드를 호출한 시점에 메서드를 호출한 스레드가 대기하게 되는 blocking 현상이 발생한다.
- callback 기반 방식을 활용하면 blocking을 피할 수 있다.
````
SimpleHttpRequest httpGet = SimpleRequestBuilder.get("https://naver.com").build();  
client.execute(httpGet, new FutureCallback<>() {  
    @Override
    public void completed(SimpleHttpResponse result) {
        // 응답 수신을 완료했을 때의 동작
    }

    @Override
    public void failed(Exception ex) {
        // 요청을 보내는데 실패했을 때의 동작
    }

    @Override
    public void cancelled() {
        // 요청이 취소되었을 때의 동작
    }
});
````

### AsyncHttpRequestRetryExec
- 수신한 HTTP 응답 상태 코드가 TOO_MANY_REQUESTS (429) 혹은 SERVICE_UNAVAILABLE (503)이라면 요청을 재시도한다. 
- 기본적으로 1초 동안의 지연 이후 최대 1회까지 재시도를 수행한다. 재시도 횟수와 지연 시간은 다음과 같이 HttpRequestRetryStrategy 설정을 추가해 변경할 수 있다.
````
// 5회까지 수행 5초 동안 지연
HttpRequestRetryStrategy retryStrategy = new DefaultHttpRequestRetryStrategy(5, TimeValue.ofSeconds(5L));  
CloseableHttpAsyncClient client = HttpAsyncClients.custom()  
        .setRetryStrategy(retryStrategy)
        .build();
````
- https://d2.naver.com/helloworld/0881672
