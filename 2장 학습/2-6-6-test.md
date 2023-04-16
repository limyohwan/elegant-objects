```java
import java.util.concurrent.Callable;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

//2-6-6 thread safety test 예제
public class Test {
    public static void main(String[] args) {
        final Cash cash = new Cash(15, 10);
        final CountDownLatch start = new CountDownLatch(1);
        final Callable<Object> script = new Callable<Object>() {
            @Override
            public Object call() throws Exception {
                start.await();
                cash.mul(2);
                System.out.println(cash);
                return null;
            }
        };
        final ExecutorService svc = Executors.newCachedThreadPool();
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        svc.submit(script);
        start.countDown();
        svc.shutdown();
    }

    static class Cash {
        private int dollars;
        private int cents;

        public Cash(final int dollars, final int cents) {
            this.dollars = dollars;
            this.cents = cents;
        }

        @Override
        public String toString() {
            return String.format("%d.%d", this.dollars, this.cents);
        }

        public void mul(int factor){
            this.dollars *= factor;
            this.cents *= factor;
        }
    }
}
```