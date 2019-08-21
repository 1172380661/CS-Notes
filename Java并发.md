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
## 线程池 ##
#### 执行步骤 ####

- 当接收到一个新任务时，如果核心线程池未满则创建一个新线程来执行该任务。  
- 如果核心线程池已满，任务队列未满则将任务存入任务队列中。  
- 如果任务队列已满，线程数未达到最大线程数则创建一个新线程来执行该任务。  
- 如果都满则根据饱和策略来决定如何处理新线程。
#### 参数 ####
Java中提供了 ThreadPoolExecutor 来创建线程池。

    ThreadPoolExecutor threadPoolExecutor = 
	new ThreadPoolExecutor(10, 20, 100L, TimeUnit.SECONDS,new LinkedBlockingQueue<>());
- corePoolSize：核心线程池数量
- runnabkeTaskQueue：任务队列
- maximumPoolSize：最大线程池数量
- ThreadFactory：给线程命名的ThreadFactory
- RejectedExecuteHandler：饱和策略
- keepAliveTime：非核心线程空闲存活时间
#### 任务提交 ####

    public void execute(Runnable command)
    public <T> Future<T> submit(Callable<T> task)
	public <T> Future<T> submit(Runnable task, T result)
#### 饱和策略 ####

- AbortPolicy：直接抛出异常。
- CallerRunsPolicy：用调用者所在的线程执行该任务。
- DiscardOldestPolicy：丢弃队列里最近的一个任务，并执行当前任务。
- DiscardPolicy：不处理，直接丢弃。
#### 合理的分配线程池 ####
针对具体的任务来创建不同的线程池

- 如果是CPU密集型任务，则应该减少创建线程的数量。
- 如果是IO密集型应该尽可能多的创建线程。
- 如果是混合型任务，应该将其拆分成俩种任务。
## Executor ##
Executor可以帮助我们来管理线程，进行线程的启动、执行和关闭。还可以避免产生 this 逃逸问题。
#### Executor框架结构 ####
![Executor框架结构](pics/776259-20160426201537486-1323529733.png)
