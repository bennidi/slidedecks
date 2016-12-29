#HSLIDE

### Agenda

- Thread management
  - Thread Lifecycle (NEW -> ... -> TERMINATED)
  - Thread organization {ThreadGroup}s
  - Thread communication (#notify,#notifyAll, #wait)
- Thread synchronization
  - Shared mutable state and the Java Memory Model
  - Synchronization techniques: Monitors, java.concurrent.*
  - 


#HSLIDE

### Thread Management

![Thread Group Hierarchy](http://images.techhive.com/images/idge/imported/article/jvw/2002/08/jw-0802-java101-100157075-orig.gif)

> Show lifecycle diagram with code breakpoints of simple example

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

- [Java Thread Groups](http://www.javaworld.com/article/2074481/java-concurrency/java-101--understanding-java-threads--part-4---thread-groups--volatility--and-threa.html)
