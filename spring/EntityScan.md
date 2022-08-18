# @EntityScan

- Entity 클래스가, Entity를 사용하고자 하는 application main package와 다른 package에 있을 때 사용
- Entity Class가 main application package나 그 하위 패키지에 있지 않을 경우
- application이 다른 패키지에 있는 Entity Class를 찾을 수 있도록 해주는 어노테이션
- @EntityScan 을 사용하면 Entity에 대한 Spring Boot 자동 구성 스캔 비활성화
````
@Configuration
@EntityScan("com.baeldung.demopackage")
public class EntityScanDemo {
    // ...
}
````
