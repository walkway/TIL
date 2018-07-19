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

### parallel
- 한 가지 작업을 서브 작업으로 나누고 -> 분리된 스레드에서 병렬 처리
- 런타임 시 하나의 작업을 서브 작업으로 자동으로 나누고, 서브 작업의 결과를 자동으로 결합해서 최종 결과물 생성
````
List<String> list = Arrays.asList("John", "Simon", "Andy", "Andrew", "Bill");
Stream<String> parallelStream = list.parallelStream();
parallelStream.forEach(ParellelExam::print)
````
