# interface

- java8 interface default method 정의 가능
````
public interface Interface {
	int a();
	default void b() {
	}

	static void c() {
	}
}
````