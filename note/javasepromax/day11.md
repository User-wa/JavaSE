

# 线程

* 线程是一个程序内部的一条执行路径。main方法就是一条单独的执行路径，如果只有一条单独的执行路径，称为单线程程序
* 多线程是指从软硬件上实现多条执行流程的技术。

## 多线程的创建

* ![image-20220831211024590](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220831211024590.png)

### 方式一：继承Thread类

![image-20220831193607409](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220831193607409.png)



* ```java
  public static void main(String[] args) {
          Thread t = new MyThread();
          t.start();
          for (int i = 0; i < 5; i++) {
              System.out.println("主线程运行中" + i);
          }
      }
  
  }
  class MyThread extends Thread{
      @Override
      public void run() {
          for (int i = 0; i < 5; i++) {
              System.out.println("子线程运行中" + i);
          }
      }
  ```

* 调用start方法，而不是调用run方法，因为操作系统会认为run为普通方法，就不会产生子线程和主线程同时发生。此时相当于还是单线程执行。一个提示

* 把主线任务放在子线程之后，不然就会直接执行完主线程。再执行t线程。

### 方式二：实现Runnable接口

![image-20220831201128921](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220831201128921.png)

* ```java
  Runnable r = new ThreadInterface();
          Thread t = new Thread(r);
          t.start();
          for (int i = 0; i < 5; i++) {
              System.out.println("主线程运行中" + i);
          }
  
      }
  }
  class ThreadInterface implements Runnable{
  
      @Override
      public void run() {
          for (int i = 0; i < 5; i++) {
              System.out.println("子线程运行中" + i);
          }
      }
  }
  ```

* 优点：扩展性得到加强，可以继续继承和实现

* 缺点：如果线程有结果不能直接返回值

* ```java
  //匿名内部类形式！！！！ 
  Runnable r = new Runnable() {
              @Override
              public void run() {
                  for (int i = 0; i < 5; i++) {
                      System.out.println("子线程1运行中" + i);
                  }
              }
          };
          Thread t = new Thread(r);
          t.start();
  
          Thread t1 = new Thread(new Runnable() {
              @Override
              public void run() {
                  for (int i = 0; i < 5; i++) {
                      System.out.println("子线程2运行中" + i);
                  }
              }
          });
          t1.start();
  
          new Thread(new Runnable() {
              @Override
              public void run() {
                  for (int i = 0; i < 5; i++) {
                      System.out.println("子线程3运行中" + i);
                  }
              }
          }).start();
  
          new Thread(() -> {for (int i = 0; i < 5; i++) {//函数式接口
              System.out.println("子线程4运行中" + i);}
          }).start();
  
          for (int i = 0; i < 5; i++) {
              System.out.println("主线程运行中" + i);
          }
  ```

### 方式三：JDK5新增，实现callable接口

* ![image-20220831212733155](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220831212733155.png)

* ```java
          Callable<String> c = new MyThread4(10);
          FutureTask<String> f = new FutureTask<>(c);
          Thread t = new Thread(f);
          t.start();
      
      	Callable<String> c2 = new MyThread4(100);
            FutureTask<String> f2 = new FutureTask<>(c2);// FutureTask继承了Runnable接口
            Thread t2 = new Thread(f2);
            t2.start();
        
            try {
                String s = f.get();//通过 FutureTask的get方法得到子线程完成后的结果
                System.out.println("结果是" + s);
            } catch (Exception e) {
                e.printStackTrace();
            }
           try {
                    String s2 = f2.get();
                    System.out.println("结果是" + s2);
                } catch (Exception e) {
                    e.printStackTrace();
                }
      
            }
        }
      
        class MyThread4 implements Callable<String>{
            private int n;
            public MyThread4(int n) {
            this.n = n;
        }
        
        @Override
        public String call() throws Exception {
            int sum = 0;
            for (int i = 1; i <= n; i++) {
                sum += i;
            }
            return "" + sum;
        }
      ```

##   Thread的常用方法

* ![image-20220831223248830](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220831223248830.png)

* 设置子线程

* ```java
   Thread t = new MyThread("1号");
  //        t.setName("1号");
          t.start();
          System.out.println(t.getName());
  
          Thread t1 = new MyThread("2号");
  //        t1.setName("2号");
          t1.start();
          System.out.println(t1.getName());
  
          Thread t3 = Thread.currentThread();//获取执行该代码的当前线程，主线程的名字为main
          t3.setName("大哥大");
          System.out.println(t3.getName());
  
          for (int i = 0; i < 5; i++) {
              System.out.println(t3.getName() + "线程输出了" + i);
          }
  ```

* ```java
  public class MyThread extends Thread{
      public MyThread() {
      }
  
      public MyThread(String name) {
          super(name);//为当前线程对象创建名称，送给父类的有参构造器初始化。
      }
  
      @Override
      public void run() {
          for (int i = 0; i < 5; i++) {
              System.out.println("子线程" + currentThread().getName() + "输出了" + i);
          }
      }
  }
  ```

* ![image-20220901074936807](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901074936807.png)

## 线程安全

### 概念，发生的原因

* 原因：存在多线程并发，同时访问共享资源，存在修改共享资源

* ```java
    Account acc = new Account();//面向对象，账户对象
          acc.setMoney(1000.0);
  //        Thread t1 = new TakeThread(acc, "小明");
  //        Thread t2 = new TakeThread(acc, "小红");
  //        t1.start();
  //        t2.start();
          new TakeThread(acc, "小明").start();
          new TakeThread(acc, "小红").start();//多线程同时出发
  ```

* ```java
   private Account acc;//创建account对象，存储。
    
      public TakeThread(Account acc,String name){
          super(name);//重写父类的name
          this.acc = acc;
      }
    
      public Account getAcc() {
          return acc;
      }
    
      public void setAcc(Account acc) {
          this.acc = acc;
      }
    
      @Override
      public void run() {
          this.acc.takeMoney(1000.0);//money为账户中的属性，可以在账户类中实现。
      }
  ```

* ```java
   private String id;
      private Double money;
    
      public String getId() {
          return id;
      }
    
      public void setId(String id) {
          this.id = id;
      }
    
      public Double getMoney() {
          return money;
      }
    
      public void setMoney(Double money) {
          this.money = money;
      }
    
      public Account(){
    
      }
    
      public Account(String id, Double money) {
          this.id = id;
          this.money = money;
      }
    
      public void takeMoney(double money){
          String name = Thread.currentThread().getName();//得到当前线程的名字
          if (this.money >= money){
              System.out.println(name + "现在有" + this.money + "元");
              this.money -= money;
              System.out.println(name + "取完钱后还剩" + this.money + "元");
          }
          else {
              System.out.println(name + "没钱，请换卡");
          }
      }
  ```


## 线程同步

* ![image-20220901174315308](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901174315308.png)

### 方式一：同步代码块

* ![image-20220901180032542](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901180032542.png)

* ```java
  //多个线程调用静态方法是用类调用的，一个一个等待
      public static void run(){
          synchronized (Account.class) {//把类锁住
  
          }
      }
  ```

* ```java
  synchronized (this) {//实例方法通过this进行加锁，实现每一个用户都用不同的锁
      if (this.money >= money){
          System.out.println(name + "现在有" + this.money + "元");
          this.money -= money;
          System.out.println(name + "取完钱后还剩" + this.money + "元");
      }
      else {
          System.out.println(name + "余额不足");
      }
  }
  ```

### 方式二：同步方法

* ![image-20220901181704789](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901181704789.png)

* ```java
  public synchronized void takeMoney(double money){ //直接加synchronized修饰方法，变成同步方法
      String name = Thread.currentThread().getName();
      //实例方法通过this进行加锁，实现每一个用户都用不同的锁
      if (this.money >= money){
          System.out.println(name + "现在有" + this.money + "元");
          this.money -= money;
          System.out.println(name + "取完钱后还剩" + this.money + "元");
      }
      else {
          System.out.println(name + "余额不足");
      }
  
  }
  ```

### 方式三：Lock锁

* ​	![image-20220901182848148](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901182848148.png)

* ```java
  //一个对象只有一把锁，而且不能撬锁
      private final Lock lock = new ReentrantLock();
  
      lock.lock();
      try {
          if (this.money >= money){
              System.out.println(name + "现在有" + this.money + "元");
              this.money -= money;
              System.out.println(name + "取完钱后还剩" + this.money + "元");
          }
          else {
              System.out.println(name + "余额不足");
          }
      } finally {
          lock.unlock();//防止因异常而使得锁变成死锁，让线程无限等待
      }
  ```

## 线程通信

* ```java
  //锁是可以跨方法的，用的是同一个账户的锁。假设亲爹用了，其它都被锁拒之门外。
  public synchronized void saveMoney(double money) {
          String name = Thread.currentThread().getName();
          //实例方法通过this进行加锁，实现每一个用户都用不同的锁
          try {
              if (this.money == 0){
                  this.money += money;
                  System.out.println(name + "现在存入" + this.money + "元");
                  this.notifyAll();//唤醒其他所以线程
                  this.wait();//锁对象，让自己休眠，当前进程进入等待。
              }
              else {
                  this.notifyAll();
                  this.wait();
              }
          } catch (Exception e) {
              e.printStackTrace();
          }
  ```

* ```java
   @Override
      public void run() {
          while (true) {
              this.acc.saveMoney(1000.0);
              try {
                  Thread.sleep(2000);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
          }
      }
  ```

* ```java
  Account acc = new Account("IDMM-11", 0.0);
          //创建两个孩子线程来取钱
          new TakeThread(acc,"小明").start();
          new TakeThread(acc,"小红").start();
  //        Thread t1 = new TakeThread(acc, "小明");
  //        Thread t2 = new TakeThread(acc, "小红");
  //        t1.start();
  //        t2.start();
          //创建三个父亲来存钱
          new SaveThread(acc, "亲爹").start();
          new SaveThread(acc, "干爹").start();
          new SaveThread(acc, "岳父").start();
  ```

* ![image-20220901195325312](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901195325312.png)

* 只有锁对象知道谁在用它，谁在等待它。一个桥梁。

## 线程池（重点）

### 线程池的概述

* 线程池就是一个可以复用线程的技术，防止因创建大量线程而使得系统性能降低。

* ![image-20220901203039850](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901203039850.png)

* 工作线程等待前面的线程执行完毕后才执行任务队列的下一个线程。

### 线程池实现的API，参数说明

* ![image-20220901204803302](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901204803302.png)

### 线程池处理Runnable任务

* ![image-20220901211133543](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901211133543.png)

* ![image-20220901211143747](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901211143747.png)

* ![image-20220901211338592](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220901211338592.png)

* ```java
  //public ThreadPoolExecutor(int corePoolSize,
      //                              int maximumPoolSize,
      //                              long keepAliveTime,
      //                              TimeUnit unit,
      //                              BlockingQueue<Runnable> workQueue,
      //                              ThreadFactory threadFactory,
      //                              RejectedExecutionHandler handler) {
  ExecutorService pool = new ThreadPoolExecutor(4, 6, 1,
         TimeUnit.SECONDS, new ArrayBlockingQueue<>(3), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy() );
          Runnable r = new MyThread();
          pool.execute(r);
          pool.execute(r);
          pool.execute(r);
          pool.execute(r);
  
          pool.execute(r);
  
          pool.execute(r);
          pool.execute(r);
          pool.execute(r);
          pool.execute(r);
          pool.execute(r);
  ```


### 线程池处理Callable任务

* ![image-20220902101119620](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220902101119620.png)

* ```java
  ExecutorService pool = new ThreadPoolExecutor(4, 6, 1,
                  TimeUnit.SECONDS, new ArrayBlockingQueue<>(3), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
          Future rs1 = pool.submit(new MyThread(100));//得到Future对象！
          Future rs2 = pool.submit(new MyThread(200));
          Future rs3 = pool.submit(new MyThread(300));
          Future rs4 = pool.submit(new MyThread(400));
          System.out.println(rs1.get());
          System.out.println(rs2.get());//会等待完成结果后才得到返回值！
          System.out.println(rs3.get());
          System.out.println(rs4.get());
   //繁琐！！！！
  //        System.out.println(pool.submit(new MyThread(200)).get());
  //        System.out.println(pool.submit(new MyThread(300)).get());
  ```

* ```java
  public class MyThread implements Callable {
      private int n;
  
      public MyThread() {
      }
  
      public MyThread(int n) {
          this.n = n;
      }
  
      @Override
      public Object call() throws Exception {
          int sum = 0;
          for (int i = 1; i <= n; i++) {
              sum += i;
          }
          return Thread.currentThread().getName() + "完成了" + n + "的求和,结果是：" + sum;
      }
  }
  ```

### Executors工具类实现线程池

* ![image-20220902102840602](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220902102840602.png)

* ![image-20220902104426017](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220902104426017.png)

* ```java
   ExecutorService pool = Executors.newFixedThreadPool(3);
  ```

## 补充知识：定时器

* ![image-20220903085705463](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903085705463.png)

* ```java
  		Timer t = new Timer();
  
          t.schedule(new TimerTask() {//匿名内部类
              @Override
              public void run() {
                  System.out.println("进程AAA运行了,时间为" + new Date());
  
              }
          }, 0, 2000);
  
          t.schedule(new TimerTask() {
              @Override
              public void run() {
                  System.out.println("进程BBB运行了,时间为" + new Date());
              }
          }, 0, 2000);
  ```

* ![image-20220903090938082](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903090938082.png)

* 常用

* ```java
   ScheduledExecutorService s = Executors.newScheduledThreadPool(3);//创建ScheduledExecutorService定时器（可以不限制创建临时线程！）
  
          s.scheduleAtFixedRate(new TimerTask() {
              @Override
              public void run() {
                  System.out.println("进程AAA运行了，时间为" + new Date());
                  try {
                      Thread.sleep(10000);//多线程中这个线程玩完，不会影响其他线程
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
  
              }
          }, 0, 2, TimeUnit.SECONDS);
  
          s.scheduleAtFixedRate(new TimerTask() {
              @Override
              public void run() {
                  System.out.println("进程BBB运行了，时间为" + new Date());
                  System.out.println(10 / 0);//该线程暴毙！
              }
          }, 0, 2, TimeUnit.SECONDS);
  
          s.scheduleAtFixedRate(new TimerTask() {
              @Override
              public void run() {
                  System.out.println("进程CCC运行了，时间为" + new Date());
              }
          }, 0, 2, TimeUnit.SECONDS);
  
          s.scheduleAtFixedRate(new TimerTask() {
              @Override
              public void run() {
                  System.out.println("进程DDD运行了，时间为" + new Date());
              }
          }, 0, 2, TimeUnit.SECONDS);
  
  ```

## 补充知识：并发和并行

![image-20220903092432876](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903092432876.png)

## 补充知识：线程的生命周期（6种状态转换）

* ![image-20220903093129009](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903093129009.png)

* ![image-20220903093227610](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903093227610.png)

* ![image-20220903093326226](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903093326226.png)

