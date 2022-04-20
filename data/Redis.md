# Redis

````
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.
````
- REmote DIctionary Server
- NoSQL DBMS
  -  전통적인 관계형 데이터베이스 보다 덜 제한적인 일관성 모델을 이용하는 데이터의 저장 및 검색을 위한 매커니즘을 제공
- memcached와 같은 In memory 솔루션: 캐시 방식을 통해 DB Read의 부하를 감소시키는 방식
- key, value store
- PUT / GET Operation을 지원
- 모든 데이타는 메모리에 저장, 빠른 write/read 속도 보장
- 다양한 데이터 타입 지원 String, Bitmap, Hash, List, Set, Sorted Set, Geospatial Index, Hyperloglog, Stream
- snapshotting (RDB): 순간적으로 메모리에 있는 내용을 DISK에 전체를 옮겨 담는 방식
- Pub/Sub Model
- 메인 쓰레드(이벤트루프)는 싱글 스레드
  - 장점 : atomic 보장, race condition 회피
  - 단점 : 오래걸리는 명령을 실행하면 다른 명령에 영향
  - O(N) 명령어는 사용 자제 : KEYS, FLUSHALL, FLUSHDB, DEL

### 명령어
- 현재 키 값들 확인
````
keys *
````
- 키/값을 저장
````
set key value
````
````
set key value [NX|XX] [EX 초] [PX 밀리초]
````
- 키에 해당하는 값 조회
````
get key
````
- 삭제
````
del key
````

### 구조체
- hset(hmset)
````
hmset user id 1234 name test pw abcd uniqueNumber 1000;

hmget user id
> "1234"

hgetall user
> "id"
> "1234"
> "name"
> "test"
> "pw"
> "abcd"
> "uniqueNumber"
> "1000;"
````

### cache
- 속도가 빠른 장치와 느린 장치 사이에서 속도 차에 따른 병목 현상을 줄이기 위한 범용 메모리를 말한다.
- CPU에서는 CPU 코어(고속)와 메모리(CPU에 비해 저속) 사이에서 속도 차에 따른 병목 현상을 완화하는 역할을 한다.
- 웹 브라우저 캐시파일는 웹 페이지 상의 이미지 등을 하드디스크에 미리 저장해 두고, 다음번 웹 페이지 접근 시 해당 사이트가 아닌 하드디스크에서 이미지를 불러들여 로딩 속도를 높이는 역할을 한다. 속도 빠른 하드디스크와 상대적으로 느린 웹 페이지 가운데서 병목을 줄인다.
- 캐시의 주요 목적은 더 느린 기본 스토리지 계층에 액세스해야 하는 필요를 줄임으로써 데이터 검색 성능을 향상한다.
- 속도를 위해 용량을 절충하는 캐시는 일반적으로 데이터의 하위 집합을 일시적으로 저장한다. 완전하고 영구적인 데이터가 있는 데이터베이스와는 대조적이다.

## expire
- 메모리는 한정적인 경우가 많으므로 대부분 key에 expire를 설정 권장
- 동일한 key가 들어오는 경우 timeout이 재설정
- expire는 collection의 각 item에 개별적으로 걸리지 않고 전체 Collection에 대해서

## eviction Policy
- 사용하는 메모리가 maxmemory 에 지정한 크기보다 커지면 Redis는 사용자가 지정한 Eviction 정책에 따라 저장되어 있는 데이터를 제거한 후, 새로운 데이터를 저장한다.

### noevitcion
- maxmemory 에 도달한 상태에서 클라이언트가 새로운 데이터를 저장하려고 할 때 기존 캐시 데이터를 지우지 않고 에러를 발생

### LRU (Least Recently Used)
- 사용한지 가장 오래된 데이터부터 삭제
- 이론 상의 LRU 알고리즘을 구현하려면 메모리가 많이 필요하다. Redis에서는 maxmemory-samples 옵션에서 지정한 수의 키로 샘플링하여 LRU 알고리즘의 근사치를 계산한다. 샘플 수가 많을수록 메모리를 많이 사용하지만 알고리즘의 정밀도가 올라간다.
  - allkeys-lru: 모든 키를 대상으로 LRU 알고리즘을 적용하여 키를 삭제
  - volatile-lru: EXPIRE SET 안에 있는 키를 대상으로 LRU 알고리즘을 적용하여 키를 삭제(EXPIRE SET 안에 있는 키: 만료 기간이 설정된 키)

### random
- 무작위로 데이터 삭제
  - allkeys-random : 모든 키를 대상으로 무작위로 데이터를 삭제
  - volatile-random : EXPIRE SET 안에 있는 키를 대상으로 무작위로 데이터를 삭제

### TTL
- TTL이 짧은 데이터부터 삭제
  - volatile-ttl : EXPIRE SET 안에 있는 키를 대상으로 TTL이 짧은 데이터부터 삭제

### LFU(Least Frequently Used)
- 사용 빈도수가 가장 적은 데이터부터 삭제, 최근에 사용된 데이터라도 자주 사용되지 않는다면 제거 대상
  - allkeys-lfu : 모든 키를 대상으로 LFU 알고리즘을 적용하여 키를 삭제
  - volatile-lfu : EXPIRE SET 안에 있는 키를 대상으로 LFU 알고리즘을 적용하여 키를 삭제

### example
````
// application.properties

spring.redis.port=6379
spring.redis.host=localhost
````
````
@Configuration
@PropertySource(value = "application.properties")
@EnableRedisRepositories
public class AppConfig {

    @Value("${spring.redis.port}")
    private String port;

    @Value("${spring.redis.host}")
    private String host;

    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, Integer.parseInt(port));
    }

    @Bean
    public RedisTemplate<?, ?> redisTemplate() {
        RedisTemplate<byte[], byte[]> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory());
        return template;
    }
}
````
````
@RedisHash("people")
public class Person {

    @Id
    String id;

    String name;

    Address address;

    public Person(String name, Address address) {
        this.name = name;
        this.address = address;
    }
}

public class Address {
    String country;
    String city;

    public Address(String country, String city) {
        this.country = country;
        this.city = city;
    }
}
````
````
public interface PersonRepository extends CrudRepository<Person, String> {
}
````
````
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = AppConfig.class)
public class PersonRepositoryTest {

    @Autowired
    PersonRepository repository;

    @Test
    public void basicCrudOperations() {

        Person person = new Person("name", new Address("Korea", "Seoul"));

        Person savedPerson = repository.save(person);

        Optional<Person> findPerson = repository.findById(savedPerson.getId());

        assertThat(findPerson.isPresent()).isEqualTo(Boolean.TRUE);
        assertThat(findPerson.get().getFirstname()).isEqualTo(person.getFirstname());
    }

}
````

- https://redis.io/
- http://bcho.tistory.com/654