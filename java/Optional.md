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

### Optional not be used in arguments
````
(+) Passing an Optional result to another method, without any semantic analysis; leaving that to the method, is quite alright.
(-) Using Optional parameters causing conditional logic inside the methods is literally contra-productive.
(-) Needing to pack an argument in an Optional, is suboptimal for the compiler, and does an unnecessary wrapping.
(-) In comparison to nullable parameters Optional is more costly.
(-) The risk of someone passing the Optional as null in actual parameters.
In general: Optional unifies two states, which have to be unraveled. Hence better suited for result than input, for the complexity of the data flow.
````
https://stackoverflow.com/questions/31922866/why-should-java-8s-optional-not-be-used-in-arguments
