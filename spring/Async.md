# Async 
- in Spring boot 
- @EnableAsync 선언하여 @Async 쓰는 것을 명시
- 비동기로 수행할 메소드에 @Async 적용
- @Async에 대한 설정이 없으면 새로운 비동기 작업을 스레드 풀에서 처리하지 않고, 새로운 스레드를 매번 생성해서 작업을 수행한다.
- 스레드 풀을 bean 등록하여 자동으로 해당 스레드 풀로 작업을 넘기도록 설정한다.
````
@EnableAsync
@SpringBootApplication
public class SpringBootApplication {
    ...
}
````
````
@Configuration
@EnableAsync
public class AsyncThreadConfiguration {

	@Bean
	public Executor asyncThreadTaskExecutor() {
		ThreadPoolTaskExecutor threadPoolTaskExecutor = new ThreadPoolTaskExecutor();
		threadPoolTaskExecutor.setCorePoolSize(8);
		threadPoolTaskExecutor.setMaxPoolSize(8);
		threadPoolTaskExecutor.setMaxPoolSize(10);
		threadPoolTaskExecutor.setThreadNamePrefix("test-");
		return threadPoolTaskExecutor;
	}
}

````
````
public class TestService {
	
    @Async("asyncThreadTaskExecutor")
    public void method() throws Exception {
        // do something
    }
}
````
LinkedBlockingQueue 생성 후, core size 만큼 스레드에서 task 처리할 수 없는 경우 queue 대기, queue가 차게되면 max 사이즈만큼 스레드 생성 처리
ex) 3 개 스레드 처리하다가 밀리는 경우 10개 queue에서 대기, 추가 요청이 발생하면 최대 8개 스레드 생성 처리


````
// Methods with Return Type
@Asyncpublic 
Future<String> asyncMethodWithReturnType() {    
  System.out.println("Execute method asynchronously - " + Thread.currentThread().getName());    
  
  try {        
    Thread.sleep(5000);        
    return new AsyncResult<String>("hello world !!!!");    
  } catch (InterruptedException e) { 
  
  }    
  
  return null;
}
````
@Configuration : Spring 설정 관련 Class로 @Component 등록되어 Scanning 될 수 있다.
@EnableAsync : Spring method에서 비동기 기능을 사용가능하게 활성화 한다.
CorePoolSize : 기본 실행 대기하는 Thread의 수
MaxPoolSize : 동시 동작하는 최대 Thread의 수, queueCapacity 차는 경우 maxPoolSize 만큼 생성
QueueCapacity : MaxPoolSize 초과 요청에서 Thread 생성 요청시, 해당 요청을 Queue에 저장하는데 이때 최대 수용 가능한 Queue의 수, Queue에 저장되어있다가 Thread에 자리가 생기면 하나씩 빠져나가 동작
ThreadNamePrefix : 생성되는 Thread 접두사 지정
````
- private method 사용 불가, public method만 사용 가능
  - @Async의 동작은 AOP가 적용
  - Spring Context에서 등록된 Bean Object의 method가 호출 될 때, Spring이 확인할 수 있고 @Async가 적용된 method의 경우 Spring이 method를 가로채 다른 Thread에서 실행 시켜주는 동작 방식
  - Spring이 해당 @Async method를 가로챈 후, 다른 Class에서 호출이 가능해야 하므로, private method는 사용할 수 없음
- self-invocation(자가 호출) 불가
  - Spring Context에 등록된 Bean의 method의 호출이어야 Proxy 적용이 가능하므로, inner method의 호출은 Proxy 영향을 받지 않기에 self-invocation이 불가능
````
- QueueCapacity 초과
  - 최대 수용 가능한 Thread Pool 수와 QueueCapacity 까지 초과된 요청이 들어오면 Task를 Reject하는 Exception 발생
  - TaskRejectedException 예외 처리 필요
  
- https://spring.io/guides/gs/async-method/
