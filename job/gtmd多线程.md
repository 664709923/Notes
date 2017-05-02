
###线程的状态

>- new：一个还没有开始（未调用start方法）的线程处于此状态。 

>- runnable：线程已经在jvm中执行，但是可能在等待其他操作系统的资源，如CPU，当线程时间片用完后即处于此状态。 
  
>- blocked：等待监视器锁以进入synchronized块、方法的线程或者是在调用了wait方法之后要重新进入一个synchronized块、方法的线程处于此状态。

>- waitting：调用了以下三个方法之一：
	- 不带参数的Object.wait方法，在等其他线程调用Object.notify或notifyall方法
	- 不带参数的Thread.join方法，等待特定线程执行结束（本质上与上一个相同）
	- LockSupport.park方法

>- timed_waitting：调用了以下五个方法之一：
	- Thread.sleep
	- 带参数的Object.wait
	- 带参数的Thread.join
	- LockSupport.parkNanos
	- LockSupport.parkUntil

>- terminated：执行完成的线程

>blocked和waitting的区别可以这样理解，blocked是因为一个线程要获取一些排他锁，如进入synchronized块、方法等被动地被阻塞；而waitting是一个线程主动地去等待某些事件的发生，是主动的。



LinkedBlockingQueue
add方法：非阻塞，但是无法添加时会抛出IllegalStateException异常
offer方法：非阻塞，建议使用；带参数的offer方法可以等待指定时间
put方法：队列满时阻塞

poll：与offer对应，非阻塞；带参数的poll方法可以等待指定时间
take：队列为空时阻塞



记录一下某坑：
Thread.setUncaughtExceptionHandler方法无法catch到某个定时任务中的exception；

newSingleThreadScheduledExecutor调用scheduleAtFixedRate方法，任务中出现nullpointException时，任务被取消掉，但是线程一直存在，处于waitting状态，且thread.setUncaughtExceptionHandler无法catch到该exception。