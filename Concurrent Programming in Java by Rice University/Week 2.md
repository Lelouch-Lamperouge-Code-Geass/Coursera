## 2.1 Critical Sections

Lecture Summary: In this lecture, we learned how __critical sections__ and the isolated construct can help concurrent threads manage their accesses to shared resources, at a higher level than just using locks. When programming with threads, it is well known that the following situation is defined to be a data race error — when two accesses on the same shared location can potentially execute in parallel, with least one access being a write. However, there are many cases in practice when two tasks may legitimately need to perform concurrent accesses to shared locations, as in the bank transfer example.

With critical sections, two blocks of code that are marked as isolated, say A and B, are guaranteed to be executed in mutual exclusion with A executing before B or vice versa. With the use of isolated constructs, it is impossible for the bank transfer example to end up in an inconsistent state because all the reads and writes for one isolated section must complete before the start of another isolated construct. Thus, the parallel program will see the effect of one isolated section completely before another isolated section can start.

#### Optional Reading:

https://en.wikipedia.org/wiki/Critical_section

https://en.wikipedia.org/wiki/Atomicity_(database_systems)

https://en.wikipedia.org/wiki/Linearizability  


## 2.2 Object-Based Isolation
Lecture Summary: In this lecture, we studied __object-based isolation__, which generalizes the isolated construct and relates to the classical concept of __monitors__. The fundamental idea behind object-based isolation is that an isolated construct can be extended with a set of objects that indicate the scope of isolation, by using the following rules: __if two isolated constructs have an empty intersection in their object sets they can execute in parallel, otherwise they must execute in mutual exclusion__. We observed that implementing this capability can be very challenging with locks because a correct implementation must enforce the correct levels of mutual exclusion without entering into deadlock or livelock states. The linked-list example showed how the object set for a delete() method can be defined as consisting of three objects — the current, previous, and next objects in the list, and that this object set is sufficient to safely enable parallelism across multiple calls to delete(). The Java code sketch to achieve this object-based isolation using the PCDP library is as follows:

```java
isolated(cur, cur.prev, cur.next, () -> {
    . . . // Body of object-based isolated construct
});
```

The relationship between object-based isolation and monitors is that all methods in a monitor object, ```M1```, are executed as object-based isolated constructs with a singleton object set, ```{M1}```. Similarly, all methods in a monitor object, ```M2```, are executed as object-based isolated constructs with a singleton object set, ```{M2}``` which has an empty intersection with ```{M1}```.

#### Optional Reading:

https://en.wikipedia.org/wiki/Monitor_(synchronization)




