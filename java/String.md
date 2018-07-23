# String

### String
- Constant String Pool
- immutable
- thread safe (o)
- 새로운 값을 할당할 때마다 새로 생성
- String의 주소값이 stack에 쌓이고, Garbage Collector 호출 전까지 heap에 쌓인다.
- String에서 저장되는 문자열은 char의 배열 형태로 저장된다. private, final -> 불변

### StringBuilder
- Heap
- mutable
- thread safe (x)
- memory에 append하는 방식으로 클래스를 직접 생성하지 않는다.
- 변경 가능
- synchronization 적용 x