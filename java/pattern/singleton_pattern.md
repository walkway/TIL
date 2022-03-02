# singleton

- 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고, 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.

### Initialization on demand holder idiom
- getInstance()가 호출되기 전까지는 초기화되지 않으며, Holder 안에 선언한 인스턴스가 로딩된 시점에 한번 호출
- JVM의 class loader의 매커니즘과 class의 load 시점을 이용하여 내부 class를 생성시킴으로 thread 간의 동기화 문제 해결
````
// 예시
public class Singleton {
  
  private Singleton() {}
  
  public static Singleton getInstance() {
    return LazyHolder.INSTANCE;
  }
  
  private static class LazyHolder {
    private static final Singleton INSTANCE = new Singleton();  
  }
}
````

### anti pattern 의견
````
it is overused, introduces unnecessary restrictions in situations where a sole instance of a class is not actually required, and introduces global state into an application
````
michaelsafyan.com/tech/design/patterns/singleton