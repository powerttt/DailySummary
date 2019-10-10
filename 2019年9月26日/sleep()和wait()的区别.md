# [JAVA线程sleep和wait方法区别](<https://www.cnblogs.com/diegodu/p/7866073.html>)

sleep是线程类Thread的方法，使此线程休眠指定时间，并让出CPU，但是监控依然保持，到时候自动恢复，调用sleep不会释放对象锁，阻塞当前线程。所以当在Synchronized块中调用sleep并不会释放对象锁。

当sleep休眠时期满后并不会立即执行，会在CPU充足的情况下运行，除非有更高的优先级



wait是Object的方法，当线程执行到wait（）时，他就进入到一个和对象相关的等待池中，并且暂时释放锁，当超时时间wait(long timeout)到后还需要返回对象锁，可以调用里面的同步方法，其他线程也可访问。

wait必须要notify或者notifyAll，或者指定时间来唤醒当前等待的线程。

wait必须放在synchronized block中，否则会抛 ”java.lang.IllegalMonitorStateException“异常。



#### 其实两者都可以让线程暂停一段时间,但是本质的区别是一个线程的运行状态控制,一个是线程之间的通讯的问题





