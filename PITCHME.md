#HSLIDE

### Agenda

- Thread management
  - Thread States
  - Thread Lifecycle (NEW -> ... -> TERMINATED)
  - Thread Groups
- Thread communication (#notify,#notifyAll, #wait)
- Thread synchronization
  - Shared mutable state and the Java Memory Model
  - Synchronization techniques: Monitors, java.concurrent.*
  - 

#HSLIDE

### Thread Management: States

A thread can be in one of the following states:
- NEW A thread that has not yet started. `Thread thread = new Thread(runnable);`
- RUNNABLE A thread executing in the Java virtual machine is `thread.start();`
- BLOCKED A thread that is blocked waiting for a monitor lock  `synchronized(){makeFunOfTrump();}`
- WAITING A thread that is waiting indefinitely for another thread to perform a particular action. `monitor.wait()`
- TIMED_WAITING A thread that is waiting for another thread to perform an action for up to a specified waiting time `monitor.wait(timeout)`
- TERMINATED The thread has terminated. Either because (a) its run method returned or (b) the thread was interrupted/stopped (unexpected thread death)

> NOTE: Many 'inaccurate/wrong' diagrams on the web. Source of truths is https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.State.html

#HSLIDE

### Thread Management: Lifecycle

![Thread States:State Machine](http://booxs.biz/images/java/thread-states.png)


#HSLIDE

### Thread Management: Thread Groups

![Thread Group Hierarchy](http://images.techhive.com/images/idge/imported/article/jvw/2002/08/jw-0802-java101-100157075-orig.gif)

- When main thread dies all sub-threads will die, too
- When sub-thread is not running as daemon its parent will wait for it to `join()`


#HSLIDE

### Thread Management: Thread States (2/2)

![Thread States: State Machine](http://booxs.biz/images/java/thread-states.png)

#HSLIDE

### x86 Memory Architecture (1/2)

- RAM is slow! Modern CPUs and JIT compilers employ many techniques to avoid access to to main memory.
  - L2, L2, L3 caches to have content that is likely to be accessed close to CPU
  - Instruction pipeline reordering to reduce idle times while going to memory (hide memory latency)

#HSLIDE

### x86 Memory Architecture (1/2)

![x86 Memory Architecture](http://2.bp.blogspot.com/-aX64aN8wOTE/Tixd9Y4X-4I/AAAAAAAAAAg/FgM0HWTCbUI/s1600/cpu.png)


#HSLIDE

### Java Memory Model

- Controlling JIT optimizations by enforcing **Happens Before Relationship** (guaranteed ordering of operations that modify memory content)
- `sfence`: A store barrier forces all store instructions prior to the barrier to happen before the barrier. Store buffer is flushed to cache => program state visible to other CPUs
- `lfence`: A load barrier forces all load instructions after the barrier to happen after the barrier. Wait on the load buffer to drain for that CPU => program state exposed from other CPUs becomes visible to this CPU
- `volatile`: sfence after write, lfence before read
- `synchronized` : Employ memory locks to the whole memory subsystem. That's why its slow.


#HSLIDE

### Fixing the double checked locking idiom

```java

// Broken multithreaded version
// "Double-Checked Locking" idiom
class Foo { 
  private Helper helper = null;
  public Helper getHelper() {
    if (helper == null) 
      synchronized(this) {
        if (helper == null) 
          helper = new Helper();
      }    
    return helper;
    }
  // other functions and members...
  }

```

#HSLIDE

### Fixing the double checked locking idiom

> If the compiler inlines the call to the constructor, then the writes that initialize the object and the write to the helper field can be freely reordered if the compiler can prove that the constructor cannot throw an exception or perform synchronization.  

> 
  


#HSLIDE

### Java Code Examples


- Related Java classes: Thread, Runnable, ExecutorService, ThreadPool
- Manual Thread handling (errors included)
- Executor Service (simplified state handling)


#HSLIDE

### Thread communication

- Object.notify and spurious wakeups
http://stackoverflow.com/questions/1050592/do-spurious-wakeups-actually-happen

- Object.wait: Adds the current thread to the waiting set of the object. Releases the wait (`and then to relinquish any and all synchronization claims on this object...`)

- https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#wait(long)
- http://stackoverflow.com/questions/2536692/a-simple-scenario-using-wait-and-notify-in-java

#HSLIDE

### Thread.class

- implements Runnable



#HSLIDE

### Resources

- Thread States
  - [State Machine with Explanation](http://www.uml-diagrams.org/examples/java-6-thread-state-machine-diagram-example.html)
  - [Thread States: Oracle JavaDoc (the truth)](https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.State.html)
  - [Incorrect explanation](http://javabook1.blogspot.de/2014/01/thread-life-cycle-in-java.html) of thread life-cycle: Missing state runnable, state ready does not exists, missing state waiting

- [Java Thread Groups](http://www.javaworld.com/article/2074481/java-concurrency/java-101--understanding-java-threads--part-4---thread-groups--volatility--and-threa.html)
- [Fixing the double checked locking idiom](https://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html)
- Another wrong example: http://image.slidesharecdn.com/javathreading-150302140412-conversion-gate02/95/java-threading-12-638.jpg?cb=1425318830
- [JVM Concurrency and Memory Barriers](https://www.infoq.com/articles/memory_barriers_jvm_concurrency)

