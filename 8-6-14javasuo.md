## Java 锁

## Java锁的概念

一段synchronized的代码被一个线程执行之前，他要先拿到执行这段代码的权限，在java里边就是拿到某个同步对象的锁（一个对象只有一把锁）； 如果这个时候同步对象的锁被其他线程拿走了，他（这个线程）就只能等了（线程阻塞在锁池等待队列中）。 取到锁后，他就开始执行同步代码\(被synchronized修饰的代码）；线程执行完同步代码后马上就把锁还给同步对象，其他在锁池中等待的某个线程就可以拿到锁执行同步代码了。这样就保证了同步代码在统一时刻只有一个线程在执行。

### 解决线程同步问题

1. 在需要同步的方法的方法签名中加入synchronized关键字

2. 使用synchronized块对需要进行同步的代码段进行同步。

3. 使用JDK 5中提供的java.util.concurrent.lock包中的Lock对象。

也就是说Java并发编程需要考虑的是多线程的同步机制，而这里的同步机制是靠锁的概念来控制的。说到线程同步就先了解一下实现线程同步机制的几种方法。

#### 1.同步方法

即有synchronized关键字修饰的方法。  由于java的每个对象都有一个内置锁，当用此关键字修饰方法时， 内置锁会保护整个方法。在调用该方法前，需要获得内置锁，否则就处于阻塞状态。

```
public synchronized void save(){}
```

注： synchronized关键字也可以修饰静态方法，此时如果调用该静态方法，将会锁住整个类

#### 2.同步代码块

即有synchronized关键字修饰的语句块。 被该关键字修饰的语句块会自动被加上内置锁，从而实现同步

```
private final Object MUTEX = new Object();
public void sync(){
    synchronized(MUTEX){
    ...
    }
}
```

注：同步是一种高开销的操作，因此应该尽量减少同步的内容。 通常没有必要同步整个方法，使用synchronized代码块同步关键代码即可

```
同步方法和同步代码块的实现
    /**
     * 线程同步的运用
     * 
     * 
     * 
     */
    public class SynchronizedThread {

        class Bank {

            private int account = 100;

            public int getAccount() {
                return account;
            }

            /**
             * 用同步方法实现
             * 
             * @param money
             */
            public synchronized void save(int money) {
                account += money;
            }

            /**
             * 用同步代码块实现
             * 
             * @param money
             */
            public void save1(int money) {
                synchronized (this) {
                    account += money;
                }
            }
        }

        class NewThread implements Runnable {
            private Bank bank;

            public NewThread(Bank bank) {
                this.bank = bank;
            }

            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    // bank.save1(10);
                    bank.save(10);
                    System.out.println(i + "账户余额为：" + bank.getAccount());
                }
            }

        }

        /**
         * 建立线程，调用内部类
         */
        public void useThread() {
            Bank bank = new Bank();
            NewThread new_thread = new NewThread(bank);
            System.out.println("线程1");
            Thread thread1 = new Thread(new_thread);
            thread1.start();
            System.out.println("线程2");
            Thread thread2 = new Thread(new_thread);
            thread2.start();
        }

        public static void main(String[] args) {
            SynchronizedThread st = new SynchronizedThread();
            st.useThread();
        }

    }
```

#### 3.使用特殊域变量\(volatile\)实现线程同步

a.volatile关键字为域变量的访问提供了一种免锁机制，

b.使用volatile修饰域相当于告诉虚拟机该域可能会被其他线程更新，

c.因此每次使用该域就要重新计算，而不是使用寄存器中的值

d.volatile不会提供任何原子操作，它也不能用来修饰final类型的变量

```
/只给出要修改的代码，其余代码与上同
        class Bank {
            //需要同步的变量加上volatile
            private volatile int account = 100;

            public int getAccount() {
                return account;
            }
            //这里不再需要synchronized 
            public void save(int money) {
                account += money;
            }
        ｝
```

注：多线程中的非同步问题主要出现在对域的读写上，如果让域自身避免这个问题，则就不需要修改操作该域的方法。  用final域，有锁保护的域和volatile域可以避免非同步的问题。

#### 4.使用重入锁实现线程同步

在JavaSE5.0中新增了一个java.util.concurrent包来支持同步。  
 ReentrantLock类是可重入、互斥、实现了Lock接口的锁，  
 它与使用synchronized方法和快具有相同的基本行为和语义，并且扩展了其能力

ReenreantLock类的常用方法有：

```
ReentrantLock\(\) : 创建一个ReentrantLock实例   
 lock\(\) : 获得锁   
 unlock\(\) : 释放锁   
```

注：ReentrantLock\(\)还有一个可以创建公平锁的构造方法，但由于能大幅度降低程序运行效率，不推荐使用。

```
//只给出要修改的代码，其余代码与上同
        class Bank {

            private int account = 100;
            //需要声明这个锁
            private Lock lock = new ReentrantLock();
            public int getAccount() {
                return account;
            }
            //这里不再需要synchronized 
            public void save(int money) {
                lock.lock();
                try{
                    account += money;
                }finally{
                    lock.unlock();
                }

            }
        ｝
```

注：关于Lock对象和synchronized关键字的选择：

```
    a.最好两个都不用，使用一种java.util.concurrent包提供的机制，能够帮助用户处理所有与锁相关的代码。 

    b.如果synchronized关键字能满足用户的需求，就用synchronized，因为它能简化代码 

    c.如果需要更高级的功能，就用ReentrantLock类，此时要注意及时释放锁，否则会出现死锁，通常在finally代码释放锁 。
```

#### 5.使用局部变量实现线程同步

```
如果使用ThreadLocal管理变量，则每一个使用该变量的线程都获得该变量的副本， 
```

副本之间相互独立，这样每一个线程都可以随意修改自己的变量副本，而不会对其他线程产生影响。

```
ThreadLocal 类的常用方法

ThreadLocal\(\) : 创建一个线程本地变量 

get\(\) : 返回此线程局部变量的当前线程副本中的值 

initialValue\(\) : 返回此线程局部变量的当前线程的"初始值" 

set\(T value\) : 将此线程局部变量的当前线程副本中的值设置为value
```

```
//只改Bank类，其余代码与上同
        public class Bank{
            //使用ThreadLocal类管理共享变量account
            private static ThreadLocal<Integer> account = new ThreadLocal<Integer>(){
                @Override
                protected Integer initialValue(){
                    return 100;
                }
            };
            public void save(int money){
                account.set(account.get()+money);
            }
            public int getAccount(){
                return account.get();
            }
        }
```

注：ThreadLocal与同步机制

```
    a.ThreadLocal与同步机制都是为了解决多线程中相同变量的访问冲突问题。 

    b.前者采用以"空间换时间"的方法，后者采用以"时间换空间"的方式
```

#### 6.使用阻塞队列实现线程同步

前面5种同步方式都是在底层实现的线程同步，但是我们在实际开发当中，应当尽量远离底层结构。  
    使用javaSE5.0版本中新增的java.util.concurrent包将有助于简化开发。  
    本小节主要是使用LinkedBlockingQueue&lt;E&gt;来实现线程的同步  
    LinkedBlockingQueue&lt;E&gt;是一个基于已连接节点的，范围任意的blocking queue。  
LinkedBlockingQueue 类常用方法  
    LinkedBlockingQueue\(\) : 创建一个容量为Integer.MAX\_VALUE的LinkedBlockingQueue  
    put\(E e\) : 在队尾添加一个元素，如果队列满则阻塞  
    size\(\) : 返回队列中的元素个数  
    take\(\) : 移除并返回队头元素，如果队列空则阻塞

```
看了下LinkedBlockingQueue put\(\)和take的源码，在put和take的时候有锁的操作，来保证：
```

```
 import java.util.Random;
 import java.util.concurrent.LinkedBlockingQueue;

 /**
  * 用阻塞队列实现线程同步 LinkedBlockingQueue的使用
  * 
  * @author XIEHEJUN
  * 
  */
 public class BlockingSynchronizedThread {
     /**
           * 定义一个阻塞队列用来存储生产出来的商品
      */
     private LinkedBlockingQueue<Integer> queue = new LinkedBlockingQueue<Integer>();
     /**
      * 定义生产商品个数
      */
     private static final int size = 10;
     /**
      * 定义启动线程的标志，为0时，启动生产商品的线程；为1时，启动消费商品的线程
      *
      /     private int flag = 0;

     private class LinkBlockThread implements Runnable {
         @Override
         public void run() {
             int new_flag = flag++;
             System.out.println("启动线程 " + new_flag);
             if (new_flag == 0) {
                 for (int i = 0; i < size; i++) {
                     int b = new Random().nextInt(255);
                     System.out.println("生产商品：" + b + "号");
                     try {
                        queue.put(b);
                     } catch (InterruptedException e) {
                         // TODO Auto-generated catch block
                         e.printStackTrace();
                     }
                     System.out.println("仓库中还有商品：" + queue.size() + "个");
                     try {
                         Thread.sleep(100);
                    } catch (InterruptedException e) {
                         // TODO Auto-generated catch block
                         e.printStackTrace();
                    }
                 }
             } else {
                 for (int i = 0; i < size / 2; i++) {
                     try {
                         int n = queue.take();
                         System.out.println("消费者买去了" + n + "号商品");
                     } catch (InterruptedException e) {
                         // TODO Auto-generated catch block
                         e.printStackTrace();
                     }
                     System.out.println("仓库中还有商品：" + queue.size() + "个");
                     try {
                         Thread.sleep(100);
                   } catch (Exception e) {
                         // TODO: handle exception
                   }
                 }
             }
         }
     }

     public static void main(String[] args) {
         BlockingSynchronizedThread bst = new BlockingSynchronizedThread();
         LinkBlockThread lbt = bst.new LinkBlockThread();
         Thread thread1 = new Thread(lbt);
         Thread thread2 = new Thread(lbt);
         thread1.start();
         thread2.start();

     }

 }
```

注：BlockingQueue&lt;E&gt;定义了阻塞队列的常用方法，尤其是三种添加元素的方法，我们要多加注意，当队列满时：

　　add\(\)方法会抛出异常

　　offer\(\)方法返回false

　　put\(\)方法会阻塞

需要使用线程同步的根本原因在于对普通变量的操作不是原子的。

  
那么什么是原子操作呢？  
原子操作就是指将读取变量值、修改变量值、保存变量值看成一个整体来操作  
即-这几种行为要么同时完成，要么都不完成。  
  
在java的util.concurrent.atomic包中提供了创建了原子类型变量的工具类，  
使用该类可以简化线程同步。  
  
其中AtomicInteger 表可以用原子方式更新int的值，可用在应用程序中\(如以原子方式增加的计数器\)，  
但不能用于替换Integer；可扩展Number，允许那些处理机遇数字类的工具和实用工具进行统一访问。  
  
AtomicInteger类常用方法：  
AtomicInteger\(int initialValue\) : 创建具有给定初始值的新的AtomicInteger  
addAddGet\(int dalta\) : 以原子方式将给定值与当前值相加  
get\(\) : 获取当前值

```
  class Bank {
          private AtomicInteger account = new AtomicInteger(100);
  
          public AtomicInteger getAccount() {
              return account;
          }
  
          public void save(int money) {
              account.addAndGet(money);
        }
     } 
```



