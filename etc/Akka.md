# Akka

- Akka는 오픈 소스 툴킷으로, JVM 상의 동시성과 분산 애플리케이션을 단순화하는 런타임이다. Akka는 동시성을 위한 여러 프로그래밍 모델을 지원하지만, Erlang으로부터 영향을 받아 actor 기반의 동시성이 두드러진다.
- non-blocking, asynchronous, concurrent
- 액터: 독립적인 구조물 (메서드를 호출 x, 새로운 인스턴스 생성 x)

### 처리율
- 암달의 법칙: 멀티코어를 사용하는 프로그램의 속도는 프로그램 내부에 존재하는 순차적sequential 부분이 사용하는 시간에 의해서 제한된다. (프로그램이 낼 수 있는 속도의 상한이 순차적 코드가 사용하는 시간에 의해서 제한된다.)
- 아카를 사용하면 프로그램 존재하는 순차적 부분, 블로킹 콜을 최소한으로 만든다.

### 스케일 아웃
- 액터
- location transparancy: 동일한 JVM 내부에서 인스턴스의 수를 늘어나게 하는 방법 가능
- elasticity: 스케일 업/다운, 아웃/인을 자유자재로 지원하는 기능

### 모듈화
- 액터를 중심으로 시스템 설계, 독립적
- 메시지를 주고받는 방식으로 커뮤니케이션: 코드의 응집성(coherence), 느슨한 결합(loosely coupled), 캡슐화 (encapsulation)

### 액터 시스템 생성
- 모든 액터는 액터 시스템 내부에서 동작 수행
- 액터 시스템 내의 액터는 스레드 스케줄링, 구성파일, 로그 서비스 등을 공유
- 일반적으로 하나의 애플리케이션에서 하나의 액터 시스템 사용
````
ActorSystem actorSystem = ActorSystem.create("TestSystem");
````

### 액터 생성
- Props: immutable 객체, 필요한 구성요소를 담는 구성 파일 클래스
- 액터 이름: 고유한 이름 사용, UUID.randomUUID() 같이 의미는 없지만, 고유한 이름을 사용하기도 함.
- actorOf: 액터를 만들기 위한 팩토리 메서드
````
ActorRef ping = actorSystem.actorOf(Props.create(PingActor.class), "pingActor");
````

### ActorRef
- 액터 시스템에서 사용하는 액터 타입
- 액터를 가리키는 참조
- 액터 객체를 둘러 싸고 있는 것 [ ActorRef [ PingActor ] ]
- UntypedActor를 상속하는 클래스는 생성자를 호출해서 객체를 생성할 수 없음.

### 장소 투명성 (Location Transparency)
- 액터의 물리적인 위치를 알 필요가 없음
- 액터의 위치가 ActorRef를 소비하는 방식에 영향을 주지 않음

### 메시지 전송
- 일반적인 객체지향 프로그램에서는 객체를 만들고, 객체가 가지고 있는 메서드를 호출한다.
- Akka에서는 액터를 만들고, 액터에 메세지를 전송한다.
- tell: 메세지(Object), 발신자(ActorRef)
````
ping.tell("start", ActorRef.noSender());
````

### 메일박스
- 액터에 메세지를 전송하면, 액터마다 가지고 있는 메일박스에 저장된다.
- ConcurrentLinkedQueue로 구현

### 라이프 사이클
- 생성(created) - 재시작(restarted) 멈춤(stopped)
- preStart(): 생성자가 완료된 직후 실행
- preStop(): 액터 라이프 사이클 종료 직전 실행
- preRestart 재시작이 이루어지기 직전에 실행
- postRestart 재시작이 이루어진 직후에 실행

### 아카 계층 구조
- 메세지 수신
- 메세지 송신
- 다른 액터 생성 (액터가 액터를 만들면, 부모 - 자식 관계 형성)

###### Akka 시작하기