# AtomicInteger

- java.concurrent.atomic
- 원자적 연산 수행
- Lock이나 Synchronized 키워드 없이 여러 thread에서 병렬적으로 수행되어도 결과의 안전성을 보장받는다.
- compare-and-swap
- 값을 변경할 때
 - 읽었던 변수의 값을 기억
 - 변경 전에 변수의 메모리 내의 값을 확인
 - 기억한 값과 같은 경우 변경
````
AtomicInteger atomicInt = new AtomicInteger(0);
ExecutorService executor = Executors.newFixedThreadPool(2);
IntStream.range(0, 1000)
    .forEach(i -> executor.submit(atomicInt::incrementAndGet));
stop(executor);
System.out.println(atomicInt.get());    // => 1000
````