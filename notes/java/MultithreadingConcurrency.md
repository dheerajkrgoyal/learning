## Multithreading and Concurrency

### Difference Between a Process and a Thread?

A process can be thought of as an independent entity that runs in its own memory space and has its own resources, including memory, file handles, and system resources. Processes are heavyweight entities, and each process operates independently of other processes. Examples of processes include running multiple instances of a web browser or a text editor on your computer. Each instance operates as a separate process, with its own memory space and resources.

On the other hand, a thread is a lightweight unit of execution that exists within a process. Threads share the same memory space and resources as the process to which they belong. Multiple threads can exist within a single process and share resources such as memory and file handles. Threads are used to achieve concurrency within a program, allowing multiple tasks to be performed simultaneously. For example, in a web server application, multiple threads can handle incoming client requests concurrently, improving the server's responsiveness and efficiency.

#### Real-World Use Case Examples:

* Web Browser: When you open multiple tabs in a web browser, each tab typically runs as a separate process. This isolation ensures that if one tab crashes or experiences issues, it does not affect the other tabs or the browser itself. Within each tab, multiple threads handle tasks such as rendering web pages, processing user input, and downloading resources concurrently.

* Word Processing Application: In a word processing application like Microsoft Word, the application itself runs as a process. Within the application, various threads handle tasks such as user interface interactions, spell-checking, auto-saving, and printing. These threads operate concurrently, providing a smooth and responsive user experience.

### How can we create a Thread in Java?

You can create a thread instance by either extending the Thread class or implementing the Runnable interface. Once you have created a thread instance, you can start it by calling the start() method.

```java
// Define a class that extends Thread
class MyThread extends Thread {
    public void run() {
        // Code to be executed by the thread
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        // Create an instance of MyThread
        MyThread myThread = new MyThread();
        // Start the thread
        myThread.start();
    }
}
```

```java
// Define a class that implements the Runnable interface
class MyRunnable implements Runnable {
    public void run() {
        // Code to be executed by the thread
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        // Create an instance of MyRunnable
        MyRunnable myRunnable = new MyRunnable();
        // Create a Thread instance and pass MyRunnable as a parameter
        Thread thread = new Thread(myRunnable);
        // Start the thread
        thread.start();
    }
}
```

### Different States of a Thread

* New:
When a thread is created but not yet started, it is in the "New" state.
In this state, the thread has been instantiated, but the start() method has not been called.

* Runnable:
After calling the start() method, the thread moves to the "Runnable" state.
In this state, the thread is eligible to run, but the scheduler has not yet selected it to be the running thread.

* Running:
When the thread scheduler selects a thread from the "Runnable" state for execution, it enters the "Running" state.
In this state, the thread's code is actively being executed by the CPU.

* Blocked (or Waiting):
A thread enters the "Blocked" state when it is temporarily inactive, typically because it is waiting for a resource or condition to become available.
For example, a thread may be blocked while waiting for I/O operations to complete or waiting to acquire a lock.

* Timed Waiting:
Threads can also enter a "Timed Waiting" state, where they pause execution for a specified period.
This state occurs when a thread calls methods such as sleep() or join() with a specified timeout.

* Terminated:
The final state of a thread is "Terminated."
This state indicates that the thread has completed its execution and will not run further.

### What Is a Daemon Thread and how to create a Daemon Thread? What are the real use cases of Daemon thread?

A daemon thread in Java is a special type of thread that runs in the background, providing services to other threads or performing tasks that are not critical to the application's lifecycle. Unlike user threads, daemon threads do not prevent the JVM from exiting when all user threads have finished executing. They are automatically terminated when all non-daemon threads have exited or when the JVM shuts down.

#### Real-World Use Cases:

Garbage Collection: Daemon threads are commonly used by the JVM for tasks such as garbage collection. The JVM creates a daemon thread to perform garbage collection in the background, ensuring that unused memory is reclaimed while the application continues to execute.

Monitoring Services: Daemon threads can be used to monitor system resources or perform periodic maintenance tasks in the background. For example, a daemon thread might monitor disk usage, network traffic, or system performance metrics without interfering with the main application's execution.

Background Tasks: In server-side applications, daemon threads can be used to handle background tasks such as logging, sending periodic heartbeats to a monitoring system, or cleaning up temporary files. These tasks are essential for the application's operation but do not require dedicated user threads.

```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        Thread daemonThread = new Thread(() -> {
            while (true) {
                System.out.println("Daemon thread is running...");
                try {
                    Thread.sleep(1000); // Simulate some task
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        daemonThread.setDaemon(true); // Set the thread as daemon
        daemonThread.start(); // Start the daemon thread
    }
}
```

### Difference Between the Runnable and Callable Interfaces

* Return Value:
    * The Runnable interface does not return a result when its task completes. Its run() method has a void return type.
    * The Callable interface, on the other hand, can return a result when its task completes. It defines a call() method that returns a result of a specified type.

* Exception Handling:
    * In the Runnable interface, any checked exceptions thrown by the run() method must be caught and handled within the method itself.
    * The Callable interface allows the call() method to throw checked exceptions. These exceptions can be caught and handled by the calling code.

* Usage with Executors:
    * The Runnable interface is primarily used with the Executor framework for executing tasks asynchronously in a thread pool.
    * The Callable interface is used with the ExecutorService framework, which provides additional features such as the ability to submit tasks that return values and handle exceptions.

### Java Memory Model

The Java Memory Model (JMM) defines how Java programs interact with memory, including how threads interact through memory when executing concurrently.

Each Java thread operates with its own thread stack, which maintains the call stack – a record of invoked methods leading to the current execution point. Local variables for each method reside in the thread stack and are exclusive to that thread.

In contrast, the heap stores all objects created within the Java application, irrespective of the creating thread. This includes object versions of primitive types. Whether an object is assigned to a local or a member variable, it's stored in the heap.

### Data Race Condition and Visibility problem

* Data Race Condition:
A data race condition occurs when two or more threads access shared data concurrently, and at least one of the threads modifies the data. This can lead to unpredictable behavior and incorrect results due to the non-deterministic interleaving of thread operations. Data races can occur when threads access shared variables without proper synchronization, leading to inconsistent or corrupted data.

* Visibility Problem:
The visibility problem refers to the issue where changes made by one thread to shared variables may not be immediately visible to other threads. In Java, each thread has its own cache of variables, and changes made by one thread may not be immediately propagated to other threads' caches. This can result in one thread not seeing the most recent value of a shared variable, leading to incorrect behavior.

* Real-World Use Case:
Consider a scenario where multiple threads are updating a shared counter variable. Without proper synchronization, such as using locks or volatile variables, data races can occur. For instance, one thread may read the counter's value while another thread is in the process of updating it, leading to inconsistent or incorrect results. Additionally, if one thread updates the counter, other threads may not immediately see the updated value due to visibility issues.

```java
public class Counter {
    private int count;

    public void increment() {
        count++; // Not thread-safe
    }

    public int getCount() {
        return count; // Not thread-safe
    }
}
```

### Difference between Volatile, Atomic and Synchronized

* Volatile: The volatile keyword in Java is used to indicate that a variable's value may be modified by multiple threads, and changes to the variable should be immediately visible to other threads. However, it does not provide atomicity or mutual exclusion. Instead, it ensures that reads and writes to the variable are not reordered by the compiler or processor, thereby addressing the visibility problem.

* Atomic: The java.util.concurrent.atomic package provides classes like AtomicInteger, AtomicLong, and AtomicReference, which offer atomic operations on variables without the need for explicit synchronization. These classes use compare-and-set (CAS) operations to ensure that updates to the variables are performed atomically and without data races. They are suitable for scenarios where operations like incrementing or updating variables need to be thread-safe.

* Synchronized: The synchronized keyword in Java is used to create a mutually exclusive block of code, known as a synchronized block, which can only be executed by one thread at a time. It ensures that only one thread can execute the synchronized block, while other threads are blocked until the lock is released. Synchronization provides both atomicity and mutual exclusion, ensuring thread safety by preventing data races and ensuring consistent access to shared resources.

#### Real-World Use Cases:

* Volatile: Use volatile when you have a variable that is shared among multiple threads, and you want to ensure that changes to the variable's value are immediately visible to other threads without the need for locking. For example, a flag indicating whether a thread should continue running. Usecase: Flags

* Atomic: Use atomic variables when you need to perform atomic operations like incrementing, updating, or comparing variables in a thread-safe manner. For example, maintaining a global counter or implementing thread-safe caching mechanisms. Usecase: Counters

* Synchronized: Use synchronized blocks when you need to ensure that only one thread can access a block of code or modify shared resources at a time. For example, updating shared data structures or performing critical operations that require exclusive access. Usecase: Critical sections

### Deadlock, Livelock, and Starvation conditions in Java Multithreading

#### Deadlock

Deadlock occurs when two or more threads are permanently blocked, waiting for each other to release resources, leading to a circular dependency.
* Example:
Thread A locks Resource 1 and waits for Resource 2.
Thread B locks Resource 2 and waits for Resource 1.
Both threads wait indefinitely → Deadlock occurs.
In database transactions, two queries trying to lock rows in different order can cause deadlocks.

* Prevention Strategies:
✅ Consistent Lock Ordering: Always acquire locks in a fixed order (e.g., always lock Resource 1 → Resource 2 instead of randomly locking resources).
✅ Timeouts: Use tryLock() with a timeout to avoid indefinite waiting.
✅ Deadlock Detection: Implement a resource hierarchy or use a tool like ThreadMXBean in Java to detect deadlocks.

#### Livelock

Livelock occurs when threads keep changing states in response to each other without making any actual progress.

* Example
Two people trying to pass through a narrow hallway.
Both step aside at the same time, then move back, repeating indefinitely.
The system is not blocked but also not progressing.
In distributed systems, multiple nodes retrying conflict resolution can lead to infinite state changes without progress.

* Prevention Strategies:
✅ Random Delays: Introduce a random wait time before retrying.
✅ Priority Inversion Handling: Ensure a fallback mechanism to break the cycle.
✅ Use Message Passing: Instead of retries, use a queue-based or event-driven approach to avoid unnecessary state changes.

#### Starvation

Starvation happens when a thread is perpetually denied access to resources because other high-priority threads keep executing.

* Example
A low-priority thread is never scheduled because higher-priority threads keep running.
This happens in priority-based scheduling or biased locking scenarios.
In web servers, high-priority API requests may dominate CPU cycles, starving low-priority background tasks.

* Prevention Strategies:
✅ Fair Scheduling Policies: Use ReentrantLock(true) for fair access to locks.
✅ Priority Adjustments: Dynamically adjust thread priorities if a thread is waiting too long.
✅ Avoid Starvation-Prone Algorithms: Implement fair resource allocation mechanisms like FIFO-based locks.

### Java Executor Service and type of threadpools which can be created with Java Executor service

The Java Executor Service is a higher-level concurrency utility that provides a simplified interface for managing threads and executing tasks asynchronously. It abstracts away the complexity of thread management, making our lives as developers much easier.

#### Types of Thread Pool

* FixedThreadPool

    * Description: A FixedThreadPool maintains a fixed number of threads in the pool. If a task is submitted when all threads are busy, it is queued until a thread becomes available.
    * Use Case: Suitable for scenarios where you have a predictable number of tasks and want to limit the maximum number of concurrent threads.

* CachedThreadPool

    * Description: A CachedThreadPool dynamically adjusts the number of threads based on the workload. Threads are created as needed and reused if available. Unused threads are terminated after a specified idle timeout.
    * Use Case: Ideal for scenarios with a large number of short-lived tasks or when the workload varies over time.

* SingleThreadPool

    * Description: A SingleThreadPool maintains a single thread in the pool, ensuring that tasks are executed sequentially in the order they were submitted. If the thread terminates due to an exception, a new one is created to replace it.
    * Use Case: Useful for scenarios where tasks must be executed sequentially or when you want to decouple task execution from the main application thread.

* ScheduledThreadPool

    * Description: A ScheduledThreadPool is used for executing tasks at a specific time or with a fixed delay between executions. It maintains a pool of threads and supports scheduling tasks with methods like schedule(), scheduleAtFixedRate(), and scheduleWithFixedDelay().
    * Use Case: Ideal for scenarios requiring periodic execution of tasks, such as scheduling background tasks or running recurring jobs.

```java
// Fixed Thread Pool
// Create a FixedThreadPool with 3 threads
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);

System.out.println("\nFixed Thread Pool:");
// Submit tasks to the FixedThreadPool
for (int i = 1; i <= 5; i++) {
    final int taskId = i;
    fixedThreadPool.execute(() -> {
        // Task execution
        System.out.println("Fixed Thread Task " + taskId +
                " executed by Thread: " +
                Thread.currentThread().getName());
    });
}

// Cached Thread Pool
// Create a CachedThreadPool
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();

System.out.println("\nCached Thread Pool:");
// Submit tasks to the CachedThreadPool
for (int i = 1; i <= 5; i++) {
    final int taskId = i;
    cachedThreadPool.execute(() -> {
        // Task execution
        System.out.println("Cached Thread Task " + taskId +
                " executed by Thread: " +
                Thread.currentThread().getName());
    });
}

// Single Thread Executor
// Create a SingleThreadExecutor
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();

System.out.println("\nSingle Thread Executor:");
// Submit tasks to the SingleThreadExecutor
for (int i = 1; i <= 5; i++) {
    final int taskId = i;
    singleThreadExecutor.execute(() -> {
        // Task execution
        System.out.println("Single Thread Task " + taskId +
                " executed by Thread: " +
                Thread.currentThread().getName());
    });
}

// Scheduled Thread Pool
// Create a ScheduledThreadPool with 2 threads
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(2);

// Schedule a task to execute after 5 seconds
scheduledThreadPool.schedule(() ->
        System.out.println("Scheduled Task"),
        5, TimeUnit.SECONDS);

// Schedule a task to execute every 1 second, starting immediately
scheduledThreadPool.scheduleAtFixedRate(() ->
        System.out.println("Scheduled Task"),
        0, 1, TimeUnit.SECONDS);
```



