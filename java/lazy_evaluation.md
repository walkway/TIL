# Lazy Evaluation

## Stream Lazy Evaluation
````
Intermediate operations return a new stream. They are always lazy; executing an intermediate operation such as filter() does not actually perform any filtering, but instead creates a new stream that, when traversed, contains the elements of the initial stream that match the given predicate. Traversal of the pipeline source does not begin until the terminal operation of the pipeline is executed.
````
- 필요한 순간 연산
````
List<Integer> values = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
 
values.stream() // 소스 스트림
    .filter(i -> i < 6) //1 중간 연산
    .filter(i -> i % 2 == 0) //2 중간 연산
    .map(i -> i * 10) // 3 // 중간 연산
    .forEach(System.out::println); // 종단 연산

1번 false -> 2번, 3번 연산 수행 x
1번 true -> 2번 연산 false -> 3번 연산 수행 x
-> 불필요한 연산은 수행하지 않음
````

example
````
public static void main(String[] args) {

  // 2, 3 getVeryExpensiveValue 연산은 printIfValidIndex 함수 조건에 따라서 불필요하지만 실행된다.
  long start = System.currentTimeMillis();
  printIfValidIndex(0, getVeryExpensiveValue());//1
  printIfValidIndex(-1, getVeryExpensiveValue());//2 
  printIfValidIndex(-2, getVeryExpensiveValue());//3

  // lazy evaluation
  long startSupplier = System.currentTimeMillis();
  printIfValidIndex(0, () -> getVeryExpensiveValue());
  printIfValidIndex(-1, () -> getVeryExpensiveValue());
  printIfValidIndex(-2, () -> getVeryExpensiveValue());
}

private static String getVeryExpensiveValue() {
  try {
    TimeUnit.SECONDS.sleep(3);
  } catch (InterruptedException e) {
    e.printStackTrace();
  }
  return "test";
}

private static void printIfValidIndex(int num, String value) {
  if (num >= 0) {
    System.out.println("valid");
  } else {
    System.out.println("invalid");
  }
}

private static void printIfValidIndex(int num, Supplier<String> valueSupplier) {
  if (num >= 0) {
    System.out.println("valid");
  } else {
    System.out.println("invalid");
  }
}
````