# Sealed

- Java 17 추가, 무분별한 상속을 막기 위한 목적으로 등장한 기능으로 지정한 클래스 외 상속을 허용하지 않으며, 지정한 클래스 외 상속 불가능
- 상속/구현하는 클래스는 final, non-sealed, sealed 중 하나로 선언
- Permitted Subclass들은 동일한 module에 속해야 하며 이름이 지정되지 않은 module에 선언 시에는 동일한 package 내에 속함
````
public sealed interface Vehicle permits Car, Truck {
	
}

public final class Car implements Vehicle {
	
}

public non-sealed class Truck implements Vehicle {
	
}
````
https://www.baeldung.com/java-sealed-classes-interfaces
