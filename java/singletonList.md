# Collections.singletonList()

- immutable
- 1개의 요소만 추가한 리스트를 만드는 경우 사용
- 리스트 요소를 추가, 삭제하는 경우 UnsupportedOperationException 발생
- https://stackoverflow.com/questions/26027396/arrays-aslist-vs-collections-singletonlist

### immutable, thread-safe

- immutable 객체는 생성되면 더 이상 수정되지 않고, 모든 thread에서 같은 상태의 객체를 볼 수 있다.
- read-only
- 경합 조건은 멀티 스레드가 공유 자원을 수정할 때, 발생한다. 스레드가 공유 자원에서 읽기 작업만 시도한다면 Race condition은 발생하지 않는다.
- http://tutorials.jenkov.com/java-concurrency/thread-safety.html