## 1.1 Java Threads

Lecture Summary: In this lecture, we learned the concept of threads as lower-level building blocks for concurrent programs. A unique aspect of Java compared to prior mainstream programming languages is that Java included the notions of threads (as instances of the ```java.lang.Thread``` class) in its language definition right from the start.

When an instance of ```Thread``` is created (via a ```new``` operation), it does not start executing right away; instead, it can only start executing when its start() method is invoked. The statement or computation to be executed by the thread is specified as a parameter to the constructor.

The Thread class also includes a wait operation in the form of a join() method. If thread t0 performs a t1.join() call, thread t0 will be forced to wait until thread t1 completes, after which point it can safely access any values computed by thread t1. Since there is no restriction on which thread can perform a join on which other thread, it is possible for a programmer to erroneously create a deadlock cycle with join operations. (A deadlock occurs when two threads wait for each other indefinitely, so that neither can make any progress.)

#### Further reading

https://en.wikipedia.org/wiki/Thread_(computing)  

https://docs.oracle.com/javase/tutorial/essential/concurrency/runthread.html  

https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html  

## 1.2 Structured Locks

Lecture Summary: In this lecture, we learned about __structured locks__, and how they can be implemented using ```synchronized``` statements and methods in Java. Structured locks can be used to enforce mutual exclusion and avoid data races. __A major benefit of structured locks is that their acquire and release operations are implicit, since these operations are automatically performed by the Java runtime environment when entering and exiting the scope of a synchronized statement or method, even if an exception is thrown in the middle.__

We also learned about ```wait()``` and ```notify()``` operations that can be used to block and resume threads that need to wait for specific conditions. For example, a producer thread performing an insert() operation on a bounded buffer can call wait() when the buffer is full, so that it is only unblocked when a consumer thread performing a remove() operation calls notify(). Likewise, a consumer thread performing a remove() operation on a bounded buffer can call wait() when the buffer is empty, so that it is only unblocked when a producer thread performing an insert() operation calls notify(). Structured locks are also referred to as __intrinsic locks__ or __monitors__.

#### Optional Reading

### Intrinsic Locks and Synchronization

https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html

https://docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html

https://en.wikipedia.org/wiki/Monitor_(synchronization)

## 1.3 Unstructured Locks
Lecture Summary: In this lecture, we introduced unstructured locks (which can be obtained in Java by creating instances of ReentrantLock()), and used three examples to demonstrate their generality relative to structured locks. The first example showed how explicit lock() and unlock() operations on unstructured locks can be used to support a hand-over-hand locking pattern that implements a non-nested pairing of lock/unlock operations which cannot be achieved with synchronized statements/methods. The second example showed how the tryLock() operations in unstructured locks can enable a thread to check the availability of a lock, and thereby acquire it if it is available or do something else if it is not. The third example illustrated the value of read-write locks (which can be obtained in Java by creating instances of ReentrantReadWriteLock()), whereby multiple threads are permitted to acquire a lock L in “read mode”, L.readLock().lock(), but only one thread is permitted to acquire the lock in “write mode”, L.writeLock().lock().

However, it is also important to remember that the generality and power of unstructured locks is accompanied by an extra responsibility on the part of the programmer, e.g., ensuring that calls to unlock() are not forgotten, even in the presence of exceptions.

#### Optional Reading:

https://docs.oracle.com/javase/tutorial/essential/concurrency/newlocks.html

https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/locks/Lock.html

## 1.4 Liveness and Progress Guarantees

Lecture Summary: In this lecture, we studied three ways in which a parallel program may enter a state in which it stops making forward progress. For sequential programs, an “infinite loop” is a common way for a program to stop making forward progress, but there are other ways to obtain an absence of progress in a parallel program. The first is __deadlock__, in which all threads are blocked indefinitely, thereby preventing any forward progress. The second is __livelock__, in which all threads repeatedly perform an interaction that prevents forward progress, e.g., an infinite “loop” of repeating lock acquire/release patterns. The third is __starvation__, in which at least one thread is prevented from making any forward progress.

__The term “liveness” refers to a progress guarantee.__ The three progress guarantees that correspond to the absence of the conditions listed above are _deadlock freedom, livelock freedom, and starvation freedom_.

#### Optional Reading:

https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html

https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html

https://en.wikipedia.org/wiki/Deadlock  
