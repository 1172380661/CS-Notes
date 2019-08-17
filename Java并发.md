## 线程状态 ##
- New：新建状态，创建后未执行线程的 start()方法
- Runnable：Java 将操作系统中的就绪（Ready）及运行状态（Running）状态合为 Runnable 状态
- Blocked：等待获取锁
- Waiting：线程获取某个对象的锁后，又调用了该对象的 wait 方法。 wait 方法会释放对象的锁，当其他线程调用该对象的 notify 或 notifyAll 方法后，才可以继续运行
- Timed Waiting：不需要其它线程唤醒，在超时时间到达之后会自动醒来
- Terminated：可以是线程结束任务之后自己结束，或者产生了异常而结束
## 创建线程 ##

	//实现 Runnable 接口来创建线程
	new Thread(new Runnable() {
		
		@Override
		public void run() {
			// TODO Auto-generated method stub
			
		}
	});
	//实现 callble 接口来创建线程
	new Thread(new FutureTask<String>(new Callable<String>() {

		@Override
		public String call() throws Exception {
			return null;
		}
	}));
	//继承 Thread 并实现run()方法来创建线程
## 线程常用操作 ##
#### sleep ####
sleep() 是 Thread 的一个静态方法，它可以让线程进入 Timed Waiting 状态，需要注意的是 sleep() 状态不会释放锁资源。

    public static native void sleep(long paramLong) throws InterruptedException;
#### wait、notify ####

    CachedThreadPool：一个任务创建一个线程；
    FixedThreadPool：所有任务只能使用固定大小的线程；
    SingleThreadExecutor：相当于大小为 1 的 FixedThreadPool。
