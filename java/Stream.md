# Stream

- Stream 생성
````
Stream.of
Stream.generate
List.stream
List.parallelStream
````

- 중간 조작
````
filter
map
sorted
skip
````

- 종단 조작
````
forEach
forEachOrdered
toArray
min
max
count
````
#### example
- findFirst: 스트림에서 비어있지 않은 첫번째 값을 반환
````
Optional<String> startWithS = stream.filter(s -> s.startsWith("S")).findFirst();
if (startWithS.isPresent()) {
    System.out.println("findFirst: " + startWithS.get());
}
````

- ifPresent(Consumer<? super T> consumer): 특정 결과를 반환, Optional 객체 값이 존재할 경우에 실행될 로직을 함수형 인자로 넘길 수 있음
````
Optional<String> startWithS = stream.filter(s -> s.startsWith("S"))
									.findFirst()
									.ifPresent(start -> {
												System.out.println("findFirst: " + startWithS.get());
````