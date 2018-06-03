#Thread

- notify(): 실행대기
- wait(): 일시정지
- 동기화 메소드, 동기화 블록에서 사용
````
public class WorkObject {
    public synchronized void methodA() {
        System.out.println("ThreadA의 methodA() 작업 실행");
        notify(); // 일시 정지 상태에 있는 ThreadB를 실행 대기 상태로 만듬
 
        try {
            wait(); // ThreadA를 일시 정지 상태로 만듬
        } catch (InterruptedException e) {
        }
    }
 
    public synchronized void methodB() {
        System.out.println("ThreadB의 methodB() 작업 실행");
        notify();
 
        try {
            wait();
        } catch (InterruptedException e) {
        }
    }
}
````
````
public class ThreadA extends Thread {    
    private WorkObject workObject;
    
    public ThreadA(WorkObject workObject) {
        this.workObject = workObject;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            workObject.methodA();
        }
    }
}
````
````
public class ThreadB extends Thread {
    private WorkObject workObject;
    
    public ThreadB(WorkObject workObject) {
        this.workObject = workObject;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            workObject.methodB();
        }
    }
}
````
````
public class WaitNotifyExam {
    public static void main(String[] args) {
        WorkObject workObject = new WorkObject();
 
        ThreadA threadA = new ThreadA(workObject);
        ThreadB threadB = new ThreadB(workObject);
 
        threadA.start();
        threadB.start();
    }
}
````

### Thread safe
1. 스레드마다 인스턴스 작성
- 스래드마다 인스턴스를 만들어서, 자원을 공유하지 않는다.
- 인스턴스를 스레드마다 가지는 경우, 메모리를 많이 사용하게 된다.
- 많은 스레드를 병행해서 동작시킬 경우 메모리가 부족할 수 있다.

2. synchronized 사용
- synchronized로 지정한 곳으을 배타 제어 대상으로 할 수 있다.
- 대상이 처리 중인 경우 다른 처리가 끼어들지 않도록 제한한다.

3. Atomic 클래스 사용

