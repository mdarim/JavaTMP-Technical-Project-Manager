Below is a structured **set of Java code examples** demonstrating the most important classes/interfaces from the `java.util.concurrent` package — one of the core packages in Java for writing multithreaded and concurrent code. This package includes thread-pools, task execution, synchronous utilities, concurrent collections, and more. ([Oracle Docs][1])

---

## 1️⃣ Executors & Task Submission

### **Executor & ExecutorService (Simple Thread Pool)**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        executor.submit(() -> System.out.println("Task 1 executed: " + Thread.currentThread().getName()));
        executor.submit(() -> System.out.println("Task 2 executed: " + Thread.currentThread().getName()));

        executor.shutdown(); // always shutdown
    }
}
```

✔ Executes tasks asynchronously using a fixed thread pool. ([Oracle Docs][1])

---

## 2️⃣ Callable & Future

### **Callable returns a result**

```java
import java.util.concurrent.*;

public class FutureExample {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Future<Integer> future = executor.submit(() -> {
            Thread.sleep(500);
            return 42;
        });

        System.out.println("Result from Future: " + future.get());
        executor.shutdown();
    }
}
```

✔ `Future.get()` blocks until result is ready; can cancel, check completion. ([Oracle Docs][1])

---

## 3️⃣ ScheduledExecutorService

### **Scheduling tasks**

```java
import java.util.concurrent.*;

public class ScheduledExample {
    public static void main(String[] args) {
        ScheduledExecutorService scheduler = Executors.newSingleThreadScheduledExecutor();

        scheduler.scheduleAtFixedRate(() -> System.out.println("Scheduled task"), 
                                      0, 1, TimeUnit.SECONDS);
    }
}
```

✔ Runs task periodically. ([Oracle Docs][1])

---

## 4️⃣ Synchronizers

### **Semaphore (Control access to limited resources)**

```java
import java.util.concurrent.Semaphore;

public class SemaphoreExample {
    public static void main(String[] args) {
        Semaphore sem = new Semaphore(2);

        Runnable task = () -> {
            try {
                sem.acquire();
                System.out.println("Inside semaphore: " + Thread.currentThread().getName());
            } catch (InterruptedException e) {
            } finally {
                sem.release();
            }
        };

        new Thread(task).start();
        new Thread(task).start();
        new Thread(task).start();
    }
}
```

✔ Limits concurrent threads to 2. ([Oracle Docs][2])

---

### **CountDownLatch (Wait for N tasks)**

```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchExample {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(3);

        for (int i = 1; i <= 3; i++) {
            final int id = i;
            new Thread(() -> {
                System.out.println("Worker " + id + " done");
                latch.countDown();
            }).start();
        }

        latch.await(); // wait until all workers finish
        System.out.println("All workers finished!");
    }
}
```

✔ Blocks until latch count reaches zero. ([Oracle Docs][2])

---

### **CyclicBarrier**

```java
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(3);

        for (int i = 1; i <= 3; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + " reached barrier");
                try {
                    barrier.await();
                } catch (Exception e) {}
            }).start();
        }
    }
}
```

✔ Threads wait until all reach barrier. ([Oracle Docs][2])

---

## 5️⃣ Concurrent Collections

### **ConcurrentHashMap**

```java
import java.util.concurrent.ConcurrentHashMap;

public class CHMExample {
    public static void main(String[] args) {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        map.put("a", 1);
        map.computeIfPresent("a", (k, v) -> v + 1);

        System.out.println(map.get("a"));
    }
}
```

✔ Thread-safe high-performance map. ([Oracle Docs][2])

---

### **CopyOnWriteArrayList**

```java
import java.util.concurrent.CopyOnWriteArrayList;

public class COWExample {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        list.add("Hello");
        list.add("World");
        for (String s : list) System.out.println(s);
    }
}
```

✔ Safe for concurrent reads with infrequent writes. ([Oracle Docs][2])

---

## 6️⃣ Queues

### **BlockingQueue (e.g., ArrayBlockingQueue)**

```java
import java.util.concurrent.ArrayBlockingQueue;

public class BlockingQueueExample {
    public static void main(String[] args) throws InterruptedException {
        ArrayBlockingQueue<String> q = new ArrayBlockingQueue<>(2);
        q.put("item1");
        q.put("item2");
        
        new Thread(() -> {
            try {
                System.out.println("Consumed: " + q.take());
            } catch (InterruptedException ignored) {}
        }).start();
    }
}
```

✔ Blocks on `put`/`take` for producer–consumer. ([Oracle Docs][1])

---

## 7️⃣ Fork/Join Framework

### **ForkJoinPool + RecursiveTask**

```java
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveTask;

public class ForkJoinExample {
    static class SumTask extends RecursiveTask<Integer> {
        int[] arr; int start, end;
        SumTask(int[] arr, int s, int e) { this.arr = arr; start = s; end = e; }

        protected Integer compute() {
            if (end - start <= 2) {
                int sum = 0;
                for (int i = start; i < end; i++) sum += arr[i];
                return sum;
            }
            int mid = (start + end) / 2;
            SumTask left = new SumTask(arr, start, mid);
            SumTask right = new SumTask(arr, mid, end);
            left.fork();
            return right.compute() + left.join();
        }
    }
    public static void main(String[] args) {
        ForkJoinPool pool = new ForkJoinPool();
        int[] data = {1, 2, 3, 4, 5};
        System.out.println(pool.invoke(new SumTask(data, 0, data.length)));
    }
}
```

✔ Work-stealing parallel computations. ([Oracle Docs][2])

---

## 8️⃣ TimeUnit

### **Using TimeUnit**

```java
import java.util.concurrent.TimeUnit;

public class TimeUnitExample {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Sleeping...");
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Awake!");
    }
}
```

✔ Easier time conversions and sleeps. ([Oracle Docs][2])

---

## 📌 Summary

Here are key categories in the package and examples above:

| Category                   | Classes / Interfaces                                                  |                    |
|----------------------------|-----------------------------------------------------------------------|--------------------|
| **Execution**              | `Executor`, `ExecutorService`, `ScheduledExecutorService`             |                    |
| **Tasks & Results**        | `Callable`, `Future`, `FutureTask`                                    |                    |
| **Synchronization Aids**   | `Semaphore`, `CountDownLatch`, `CyclicBarrier`, `Phaser`, `Exchanger` |                    |
| **Concurrent Collections** | `ConcurrentHashMap`, `CopyOnWriteArrayList`                           |                    |
| **Queues**                 | `BlockingQueue`, `ArrayBlockingQueue`, `ConcurrentLinkedQueue`        |                    |
| **Parallelism**            | `ForkJoinPool`, `RecursiveTask`                                       |                    |
| **Utilities**              | `TimeUnit`                                                            | ([Oracle Docs][1]) |

---

If you want **examples for additional specific classes** from this package (e.g., `Exchanger`, `Phaser`, `LinkedTransferQueue`, `ExecutorCompletionService`, or `CompletableFuture`), just let me know and I can provide those too with full runnable code!

[1]: https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/concurrent/package-summary.html?utm_source=chatgpt.com "java.util.concurrent (Java SE 12 & JDK 12 )"
[2]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html?utm_source=chatgpt.com "java.util.concurrent (Java Platform SE 8 )"
