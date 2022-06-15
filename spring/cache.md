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
