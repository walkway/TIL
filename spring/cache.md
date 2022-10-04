# Cache

````
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-cache</artifactId>
    </dependency>
</dependencies>
````

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
- cacheName(value) 값을 기준으로 캐시 저장, key와 함께 저장할 수도 있음
````
@Cacheable("bestSeller")
public Book getBestSeller(String book) {

}
````
````
cacheName
캐시 이름 (설정 메서드 리턴값이 저장되는)

value
cacheName의 alias

key
동적인 키 값을 사용하는 SpEL 표현식동일한 cache name을 사용하지만 구분될 필요가 있을 경우 사용되는 값

condition
SpEL 표현식이 참일 경우에만 캐싱 적용- or, and 등 조건식, 논리연산 가능

unless
캐싱을 막기 위해 사용되는 SpEL 표현식condition과 반대로 참일 경우에만 캐싱이 적용되지 않음

cacheManager
사용 할 CacheManager 지정 (EHCacheCacheManager, RedisCacheManager 등)

sync
여러 스레드가 동일한 키에 대한 값을 로드하려고 할 경우, 기본 메서드의 호출을 동기화즉, 캐시 구현체가 Thread safe 하지 않는 경우, 캐시에 동기화를 걸 수 있는 속성
````

### @CachePut
- 값 저장 어노테이션
- @Cacheable 유사하게 실행 결과를 캐시에 저장하지만, 조회 시에 저장된 캐시의 내용을 사용하지는 않고 항상 메소드 로직 실행

### @CacheEvict 
- cache 값 제거

### clear
````
public void clearAll() {
    for (String cacheName : cacheManager.getCacheNames()) {
        Objects.requireNonNull(cacheManager.getCache(cacheName)).clear();
    }
}
````
````
public void clearTarget(String cacheName) {
    Cache targetCache = cacheManager.getCache(cacheName);
    targetCache.clear();
}
````

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
