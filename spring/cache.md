# Cache

````
@EnableCaching
@Configuration 
public class CacheConfig {
    ... 
}
````
### @Cachable
- cache 할 수 있는 메서드를 지정
- 결과를 cache에 저장하여 뒤이은 호출에는 실제로 메서드를 실행하지 않고 저장된 값을 반환
````
@Cacheable("bestSeller")
public Book getBestSeller(String book) {

}
````
### @CachePut
- 값 저장 어노테이션
- @Cacheable 유사하게 실행 결과를 캐시에 저장하지만, 조회 시에 저장된 캐시의 내용을 사용하지는 않고 항상 메소드 로직 실행

### @CacheEvict 
- cache 값 제거

### Cache SpEL available metadata
````
#root.args: 인자는 캐시된 메소드에 배열로 전달
#root.caches: 메소드가 수행된 캐시이며 배열
#root.target: target 객체
#root.targetClass: target 객체 클래스
#root.method: 캐시된 메소드
#root.methodName: 캐시된 메소드 명
#result: 메소드 호출에서 반환된 값
````
````
@Cacheable(value="book", condition="#name.length < 32", unless="#result.hardback")
public Book findBook(String name)
````
