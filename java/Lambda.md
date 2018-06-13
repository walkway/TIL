# Lambda

- 식별자 없이 실행 가능한 함수 표현식
````
(자료형 인수, ...) -> {... 임의의 처리 ...}
````
````
( parameters ) -> expression body
( parameters ) -> { expression body }
() -> { expression body }
() -> expression body
````
````
new Thread(new Runnable(){public void run(){
    System.out.println("test");
}}).start();
````
- 인터페이스를 구현하려면 클래스를 만들고, 메서드를 정의하고, 인스턴스를 만드는 과정이 필요하다.
- 람다식은 이 과정을 간결하게 기술해준다.
````
new Thread(()->{
    System.out.println("test");
}).start();
````
- 형 추론에 의한 생략: 프로그래머가 형을 지정하지 않고, 컴파일러가 인수의 형을 추정하는 기능
- 인수가 하나인 경우의 생략: 인수가 하나인 경우, 괄호 생략
- return 문만 있는 경우 생략: 처리 본체가 return문만 있는 경우, 괄호 생략