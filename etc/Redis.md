# Redis

- REmote DIctionary Server
- NoSQL DBMS
- memcached와 같은 In memory 솔루션
- key, value store
- PUT / GET Operation을 지원
- 모든 데이타는 메모리에 저장, 빠른 write/read 속도 보장
- 다양한 데이터 타입 지원
- snapshotting (RDB): 순간적으로 메모리에 있는 내용을 DISK에 전체를 옮겨 담는 방식
- Pub/Sub Model

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

##### http://bcho.tistory.com/654