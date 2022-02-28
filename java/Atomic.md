# Atomic

### CAS (Compare And Swap) 
- 멀티 스레드 환경에서 각 CPU는 메인 메모리에서 변수값을 참조하는 것이 아닌, 각 CPU에서 캐시 영역에서 메모리를 값을 참조하게 된다. 메인 메모리에 저장된 값과 CPU 캐시에 저장된 값이 다른 경우가 있다.
- 현재 스레드에 저장된 값과 메인 메모리에 저장된 값을 비교하여 일치하는 경우 새로운 값으로 교체하고 일치하지 않는다면 실패하고 재시도를 한다. 
- 세 개의 매개변수 CAS(V, e, n)를 포함
- V 는 업데이트될 변수(메모리 값), e 는 이전 예상 값, n 은 업데이트될 예상값 
- V의 값이 e의 값과 같으면 V의 값은 n으로 설정된다.
- V의 값이 e의 값과 다르면 다른 스레드가 업데이트되었음을 ​​의미하며 현재 스레드는 아무 것도 하지 않고 현재 v의 실제 값을 반환한다.

### AtomicInteger
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

https://developpaper.com/cas-algorithm-and-java-atomic-class/