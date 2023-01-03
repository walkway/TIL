# interface
- 기본 설계도
- contract, be able to(할 수 있는), implements, 구현(클래스의 목적에 맞게 동일한 기능으로 구현)
  - interface는 contract이므로 목적을 달성하기 위한 구현에 따라 달라짐
- 인스턴스가 존재하지 않는 시점으로 스스로 초기화할 수 없음, 상수(static final)/추상 메서드(abstract method)의 집합, 다중 상속
  - 모든 멤버 변수 public static final 생략 가능
  - 모든 메소드 public abstract 생략 가능
- java8 interface default method/static 정의 가능
````
public interface Interface {
	int a();
	default void b() {
	}

	static void c() {
	}
}
````

# abstract 클래스
- 하나 이상의 추상 메서드를 가지는 클래스, 추상 메서드를 선언하여 상속을 통해서 하위 클래스에서 필수 구현
- is a kind of(한 종류), extends, 확장(자신의 기능들을 하위로 확장)
- new 키워드를 통해 직접 객체를 생성 불가(상속을 위한 클래스이기 때문), 다중 상속을 지원하지 않기 때문에 여러 개의 추상 클래스를 상속할 수는 없음
- public, protected, private 접근 제어자 모두 사용 가능

https://stackoverflow.com/questions/1913098/what-is-the-difference-between-an-interface-and-abstract-class
