# CountDownLatch

- 어떤 쓰레드가 다른 스레드에서 작업이 완료될 때까지 기다리도록 할 때 사용
````
// 초기화
CountDownLatch countDownLatch = new CountDownLatch(5);

// latch  소 
countDownLatch.countDown();

// latch 숫자가 0이 될 때까지 대기
countDownLatch.await();
````
````
public class CountDownLatchExample {

    public static void main(String[] args) throws InterruptedException {

        int maxCount = 10;
        CountDownLatch countDownLatch = new CountDownLatch(maxCount);
        ExecutorService executorService = Executors.newFixedThreadPool(maxCount);

        for (int i = 0; i < maxCount; i++) {
            executorService.submit(new Worker(i, countDownLatch));
        }

        executorService.shutdown();
        countDownLatch.await();
        System.out.println("Done awaiting");
    }

}

class Worker implements Runnable {

    private final CountDownLatch countDownLatch;
    private final int index;

    public Worker(final int index, final CountDownLatch countDownLatch) {
        this.countDownLatch = countDownLatch;
        this.index = index;
    }

    @Override
    public void run() {
        try {
            System.out.println("Start " + index);
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            System.out.println("Finish " + index);
            countDownLatch.countDown();
        }

    }
}
````