# Optional

````
class Optional<T>{
   private T t;
}
````
- str == null, NullPointException 발생
````
Optional<String> test = Optional.of(str);
````
- str == null,  비어있는 Optional 객체 반환
````
Optional<String> test = Optional.ofNullable(str);
````
- 비어있는 Optional 객체 반환
````
Optional<String> test = Optional.empty();
````
- null 확인
boolean isPresent()
````