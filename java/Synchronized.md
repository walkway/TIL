# Synchronized 블록

- 자바의 모든 객체는 락(lock)을 갖고있다.
- 모든 객체가 갖고있으니 고유락(intrinsic lock)이라고도 하고, 모니터처럼 동작한다고 하여 모니터 락(monitor lock)혹은 그냥 모니터(monitor)라고도 한다.
- 자바의 synchronized 블록은 동시성 문제를 해결하는 가장 간편한 방법으로, 고유락을 이용하여 여러 스레드의 접근을 제어한다.
````
public Class Counter {
	private int count; // 공유변수(공유자원)
    
    public int increase() {
    	return ++count; // Thread Safe하지 않은 연산
    }
}
````
- 위에서 정의한 Counter Class는 숫자를 셀 때 사용하는 클래스로, increase()를 호출할 때마다 count 변수를 1만큼 증가하고, 그 값을 반환한다.
- ++count 문은 한줄짜리 코드로 원자적(atomic)으로 동작할 것 같지만, 사실 그렇지 않다. ++연산자를 호출했을 때, 실제로는 세 가지 연산이 순차적으로 수행된다.
- 동시성 프로그래밍에서 문제가 되는 전형적인 Read - Modify - Write 패턴
````
변수를 메모리에서 읽고 (CPU의 캐시로 가져온다. read한다.)
증가시키고 (CPU가 연산한다.)
다시 메모리에 쓴다. (다시 메모리에 write한다.)
````
두 스레드가 동시에 같은 값을 읽고, 값을 증가시켜 저장하면, increase() 호출 횟수와 count값에 차이가 발생한다.
동시성 문제는 결국 여러 스레드가 공유자원으로 접근하기 때문에 발생한다. 여기서 공유자원은 count 변수이다.
동시성 문제를 해결하기 위해 count변수로 접근하는 스레드들을 제어해야한다.
````
public class Counter {
  private Object lock = new Object();
  private int count;
  public int increase() {
    synchronized(lock) {
      return ++count;
    }
  }
}
````
````
public class Counter {
  private int count;
  public int increase() {
    synchronized(this) {
      return ++count;
    }
  }
}
````
````
public class Counter {
  private int count;
  public synchronized int increase() {
    return ++count;
  }
}
````