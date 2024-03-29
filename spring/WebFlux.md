# Spring WebFlux
스프링 프레임워크에 포함된 원래 웹 프레임워크인 스프링 웹 MVC는 서블릿 API 및 서블릿 컨테이너용으로 제작되었다.
버전 5.0에 스프링 웹플럭스가 추가되었으며, 리액티브 스택 웹 프레임워크이다.
논블로킹으로 동작하고 Reactive Streams back pressure 지원하며 Netty, Undertow 및 서블릿 3.1+ 컨테이너와 같은 서버에서 실행된다.

### Spring WebFlux 왜 만들었나
1. 논블로킹 웹 스택
적은 수의 스레드로 동시성을 처리하고, 적은 하드웨어 리소스로 확장할 수 있는 논블로킹 웹 스택이 필요하여 만들어졌다. 
서블릿으로 논블로킹을 구현하려면 다른 동기처리(Filter, Servlet), 블로킹 방식(getParameter, getPart)를 쓰는 API를 사용하기 어렵다.
어떤 논블로킹과도 잘 동작하는 새 공통 API를 만들기 됐다. 이미 비동기 논블로킹 환경에서 자리 잡은 서버(e.g. Netty) 때문에라도 새 API가 필요했다.

- 논-블로킹 (non-blocking) I/O
  - 호출 직후 프로그램으로 부터 제어가 돌아옴으로서 시스템 호출 종료를 기다리지 않고 다음 처리로 넘어갈 수 있다. 
  - 프로세스가 블로킹 상태가 아니기 때문에 CPU 를 다른 프로세스에서 사용함으로서 I/O 대기시간을 줄이거나 활용할 수 있다. 
  - 이 때 발생하는 오류는 응용프로그램에서 처리하고 재시도 하는 타이밍을 따로 정의할 필요가 있다. 

2. 함수형 프로그래밍
Java 8에서 추가된 람다 표현식으로 함수형 API를 작성할 수 있게 됐다. 
continuation-style API(CompletableFuture 및 ReactiveX로 대중화됨)로 비동기 로직을 선언적으로 작성할 수 있다.
프로그래밍 모델 관점에서 웹플럭스에서 어노테이션을 선언한 컨트롤러와 함수형 웹 엔드포인트를 사용할 수 있다.

#### spring 특징
사용자의 요청마다 스레드가 필요한 Thread per request 방식 (동기 / 블록킹)
- 효율적인 사용을 위해 스레드 풀(thread pool) 사용
스레드 풀의 스레드 수 이상의 요청이 오면 Blocking Queue에서 대기
늘어난 스레드로 인한 문맥교환(Context Switching) 비용과 메모리, CPU 부하

### Reactive
변화에 반응하는 것을 중심에 두고 만든 프로그래밍 모델이다.
논블로킹은 작업을 기다리기 보단 완료 되거나 데이터를 사용할 수 있게 되면 반응하므로, 논블로킹도 리액티브이다.

### 백프레셔(back pressure)
Publisher 에서 발행하고, Subscriber에서 구독할 때, Publisher 에서 데이터를 Subscriber 로 Push 하는 방식이 아니라, Pull 방식으로 Subscriber 가 Publisher 로 처리할 수 있는 양의 크기만큼 데이터를 요청 함으로써 Subscriber의 장애를 방지하기 위함이다.
구독자가 수용할 수 있는 만큼 데이터를 요청하는 방식이다.

### Reactive API
리액티브 라이브러리는 어플리케이션 비동기 로직을 만들기 위해 풍부한 고수준 함수형 API를 제공한다.
Reactor는 스프링 웹플럭스의 리액티브 라이브러리이다.
Mono, Flux API 타입을 제공한다.
모든 연산자는 논블로킹 back pressure을 지원한다.

### Applicability
스프링 MVC/웹플럭스 사이 이분법적 사고는 좋지 않다.
지속성과 일관성을 위해 설계했으며, 함께 사용할 수 도 있다.
1. 이미 잘 동작하고 있는 스프링 MVC 어플리케이션이 있다면, 굳이 바꿀 필요 없다. 명령적(Imperative) 프로그래밍은 작성하기도, 이해하기도, 디버깅하기도 쉽다.
지금까지 대부분 블로킹 방식을 사용했기 때문에 사용할 수 있는 라이브러리가 가장 풍부하다.
2. 논블로킹 웹 스택을 알아보고 있다면, 스프링 웹플럭스는 다른 웹 스택과 같은 실행 환경을 제공하면서도, 다양한 서버(Netty, Tomcat, Jetty, Undertown, 서블릿 3.1+ 컨테이너)와 여러 리액티브 라이브러리(리액터, JxJava 등)를 지원하며, 두 가지 프로그래밍 모델(어노테이션을 선언한 컨트롤러와 함수형 웹 엔드포인트)을 사용할 수 있다.
3. Java 8 람다, 코틀린으로 개발할 수 있는 경량 함수형 웹 프레임워크를 찾는 다면, 스프링 웹플럭스 함수형 웹 엔드 포인트를 사용하면 된다. 요구사항이 덜 복잡한 소규모 어플리케이션이나 마이크로 서비스에서 좋은 선택이다.
4. 마이크로 아키텍처에서 스프링 MVC로 만든 어플리케이션과 스프링 웹플럭스 컨트롤러나 함수형 엔트포인트를 사용한 어플리케이션을 조합할 수 있다.
5. 간단하게 어플리케이션 의존성을 확인해봐도 좋다. 블로킹 방식의 영속성 API(JPA, JDBC)나 네트워크 API를 사용하고 있다면 스프링 MVC가 최소한 아키텍처를 통일할 수 있으므로 좋은 선택이다. 리액터나 RxJava로도 각 스레드에서 블로킹 API를 호출할 수 있지만, 이런 경우 논블로킹 웹스택을 거의 활용하기 어렵다.
6. 스프링 MVC 어플리케이션에서 외부 서비스를 호출한다면 WebClient를 사용해도 좋다. 스프링 MVC 컨트롤러 메소드에서 리액티브 타입(Reactor, RxJava 등)을 반환할 수 있다.
서비스 호출에 지연이 있거나 여러 서비스가 엮여 있는 API라면 효과가 좋다.
7. 팀 규모가 크다면 논블로킹, 함수형, 선언적 프로그래밍은 러닝커브가 높은 점을 고려해야 한다. 한번에 전환하지 않고 리액티브 WebClient부터 적용해보는 것도 좋은 방법이다.
굳이 전환할 필요가 없는 경우, 어떤 변화를 확인해야 할지 감이 오지 않는다면, 논블로킹 I/O 동작 방식과 효과를 학습하는 것부터 시작해라

### Performance
리액티브/논블로킹을 사용해서 바로 어플리케이션이 빨라지지 않는다. 전반적으로 보면 논블로킹 방식이 처리할 일이 많다 보니 처리 시간이 약간 더 길어질 수 있다.
리액티브와 논블로킹의 주된 이점은 고정된 적은 스레드와 적은 메모리로도 확장할 수 있는 것이다. 예측할 수 있는 방법으로 확장하기 때문에 부하 속에서 어플리케이션 복원 능력이 더 좋아진다.

#### R2DBC(Reactive Relational DataBase Connectivity)
Reactive Programming을 하는 과정에서 Database 사용이 필요한 경우에 사용한다.
JPA는 기본적으로 비동기를 제공하지 않는다. Webflux 기반에서 JPA를 사용하면 Database 부분에서 block되고, 그동안 thread가 기다리게 된다.
- Reactive 드라이버가 지원되지 않는 데이터베이스 경우 드라이버에서의 입출력에서 Blocking으로 동작하므로, WebFlux를 이용한 Reactive로 구현해도 소용이 없어진다.
R2DBC는 Blocking programming일 때보다 높은 동시성(High Concurrency)이 요구되는 상황에서 더 좋은 성능을 낸다.
동시성(Concurrency)이 높아짐에 따라 MVC-JDBC를 사용하는 것보다 Latency 감소, Throughput 증가하지만, 공식적으로 벤더사에서 제공하지 않는 경우도 있다.

### Concurrency Model
스프링 MVC와 스프링 웹플럭스는 동시성 모델과 블로킹/스레드 기본 전략이 다르다.
스프링 MVC는 어플리케이션이 처리 중인 스레드가 중단될 수 있다. 그렇기 때문에 서블릿 컨테이너는 블로킹을 대비해서 큰 스레드 풀로 요청을 처리한다.
- Thread Per Request Model
  - 브라우저 서버 Request -> Thread Pool에서 Thread 할당받아서 작업 -> 응답 후에 Thread Pool로 반환한다.
  - Thread 하나의 작업 시, Blocking
  - 시스템 부하가 높은 상태에서 Context Swiching이 발생하는 것은 스레드 데이터가 계속 로딩하는 오버헤드 문제가 발생한다.
- Thread 개수 default = 200
스프링 웹플럭스는 실행 중인 스레드가 중단되지 않는다는 전제가 있다. 논블로킹 서버는 작은 스레드 풀을 고정해 놓고 요청을 처리한다.
- 스레드를 중단하지 않는 다는 건 요청을 처리할 다른 스레드가 필요 없고, 블로킹을 대비할 필요가 없다는 의미이다.
- Event Loop 모델로 동작
- 사용자들의 요청이나 애플리케이션 내부에서 처리해야 되는 작업들은 모두 Event라는 단위로 관리되고, Event Queue에 적재되어 순서대로 처리되는 구조이다.
- Event Loop는 Event Queue에서 Event를 뽑아서 하나씩 처리한다.

### Mono, Flux
Spring Webflux에서 사용하는 reactive library가 Reactor이고 Reactor가 Reactive Streams의 구현체이다.
Mono, Flux 차이는 발행하는 데이터 수이다.
- Mono : 0 ~ 1 개의 데이터 전달
- Flux : 0 ~ N 개의 데이터 전달
두 타입 모두 리액티브 스트림 데이터 처리 프로토콜대로 onComplete 또는 OnError 시그널이 발생할 때 까지 onNext를 사용해 구독자에게 데이터를 통지한다.
모노와 플럭스 연산자는 모두 Lazy(게으른) 동작하여 subscribe를 호출하지 않으면 리액티브 사양대로 코드가 동작하지 않는다.
````
// Mono<List<String>>
여러 개의 요소가 "한번에" 반환 (Mono가 0 또는 1개이므로 List<String> 1개가 반환된다)

// Flux<String> 
여러 개의 요소가 Stream 형태로 반환
Flux<String>에서 .collectList 메서드로 Mono<List<String>>으로 형 변환 가능
````

### example
````
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-webflux -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
    <version>3.0.0</version>
</dependency>
````
````
@RestController
public class HelloController {

    @GetMapping("/")
    Flux<String> hello() {
        return Flux.just("Hello", "World");
    }

    @GetMapping("/stream")
    Flux<Map<String, Integer>> helloStream() {
        Stream<Integer> stream = Stream.iterate(0, i -> i + 1);
        return Flux.fromStream(stream)
                .map(i -> Collections.singletonMap("value", i));
    }
}
````
https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/web-reactive.html#webflux
https://godekdls.github.io/Reactive%20Spring/springwebflux/
https://howtodoinjava.com/spring-webflux/spring-webflux-tutorial/
https://medium.com/oracledevs/spring-blocking-vs-non-blocking-r2dbc-vs-jdbc-and-webflux-vs-web-mvc-900d72ee19c1 
