# 并发编程


<!--more-->

## 引入

{{< admonition >}}
一个 **注意** 横幅
{{< /admonition >}}

{{< admonition abstract >}}
一个 **摘要** 横幅
{{< /admonition >}}

{{< admonition info >}}
一个 **信息** 横幅
{{< /admonition >}}

{{< admonition tip >}}
一个 **技巧** 横幅
{{< /admonition >}}

{{< admonition success >}}
一个 **成功** 横幅
{{< /admonition >}}

{{< admonition question >}}
一个 **问题** 横幅
{{< /admonition >}}

{{< admonition warning >}}
一个 **警告** 横幅
{{< /admonition >}}

{{< admonition failure >}}
一个 **失败** 横幅
{{< /admonition >}}

{{< admonition danger >}}
一个 **危险** 横幅
{{< /admonition >}}

{{< admonition bug >}}
一个 **Bug** 横幅
{{< /admonition >}}

{{< admonition example >}}
一个 **示例** 横幅
{{< /admonition >}}

{{< admonition quote >}}
一个 **引用** 横幅
{{< /admonition >}}









## 进程与线程

### 介绍

`进程`

- 程序由指令和数据组成，但这些指令要运行，数据要读写，就必须将指令加载至 CPU ，数据加载至内存。在指令运行过程中还需要用到磁盘、网络等设备。进程就是用来加载指令、管理内存、管理 IO 的。
- 当一个程序被运行，从磁盘加载这个程序的代码至内存，这时就开启了一个进程。
- 进程就可以视为程序的一个实例。大部分程序可以同时运行多个实例进程（例如记事本、画图、浏览器等），也有的程序只能启动一个实例进程（例如网易云音乐、360安全卫士等）

`线程`

- 一个进程之内可以分为一到多个线程。
- 一个线程就是一个指令流，将指令流中的一条条指令以一定的顺序交给CPU执行。
- Java中，线程作为最小调度单位，进程作为资源分配的最小单位。在 Windows 中进程是不活动的，只是作为线程的容器。

{{< admonition info 线程与进程对比>}}

1. 进程基本上相互独立的，而线程存在于进程内，是进程的一个子集

2. 进程拥有共享的资源，如内存空间等，供其内部的线程共享

3. 进程间通信较为复杂

    - 同一台计算机的进程通信称为`IPC`（Inter-process communication）

    - 不同计算机之间的进程通信，需要通过网络，并遵循共同的协议，例如 HTTP

4. 线程通信相对简单，因为他们共享进程内的内存。例如：多个线程可以访问同一个共享变量

5. 线程更轻量，线程上下文切换成本一般上要比进程上下文切换低

{{< /admonition >}}



### 并行与并发

单核 CPU 下，线程实际还是`串行执行`的，操作系统中有一个组件叫做**任务调度器**，将 CPU 的时间片（Windows 下时间片最小约为15毫秒）分给不同的程序使用，只是由于 CPU 在线程间（时间片很短）的切换非常快，人类感觉是`同时运行的`。总结一句话就是：`微观串行，宏观并行`，一般会将这种`线程轮流使用 CPU `的做法称为**并发**，**concurrent**

| CPU  | 时间片1 | 时间片2 | 时间片3 | 时间片4 |
| :--: | :-----: | :-----: | :-----: | :-----: |
| core |  线程1  |  线程2  |  线程3  |  线程4  |

![并发](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%B9%B6%E5%8F%91.jpg "并发")

<br>

多核CPU下，每个`核（core）`都可以调度运行线程，这时候线程可以是并行的

|  CPU  | 时间片1 | 时间片2 | 时间片3 | 时间片4 |
| :---: | :-----: | :-----: | :-----: | :-----: |
| Core1 |  线程1  |  线程2  |  线程3  |  线程4  |
| Core2 |  线程1  |  线程2  |  线程3  |  线程4  |

![并行](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%B9%B6%E8%A1%8C.jpg "并行")

<br>

>引用**Rob Pike**的一段描述：
>
>- **并发（concurrent）**是同一时间应对（dealing with）多件事情的能力
>- **并行（parallel）**是同一时间动手做（doing）多件事情的能力
>
>**用例**
>
>- 家庭主妇做饭，打扫卫生，给孩子喂奶，她一个人轮流交替做这么多件事，这时就是并发
>- 家庭主妇雇佣了个保姆，她们一起做这些事，这时既有并发，也有并行（这时会产生竞争，例如锅只有一口，一个人用锅时，另一个人就得等待）
>- 雇了三个保姆，一个专做饭，一个专打扫卫生，一个专喂奶，互不打扰，这时就是并行
>
>**Rob Pike** 资料
>
>- Golang语言的创造者
>- [Rob Pike - 百科百度](https://baike.baidu.com/item/羅布·派克/10983505)

### 应用

#### 应用之异步调用

以调用角度来讲，如果

- 需要等待结果返回，才能继续运行就是同步
- 不需要等待结果返回，就能继续运行就是异步

<br>

**设计**

多线程可以让方法执行变为异步的（既不要巴巴干等着）比如说读取磁盘文件时，假设读取操作话费了5分钟，如果没有线程调度机制，这5秒CPU什么都做不了，其它代码都得暂停...

<br>

**结论**

- 比如在项目中，视频文件需要转换格式等操作比较费时，这时开启一个新线程处理视频转换，避免阻塞主线程
- tomcat的异步servlet也是类似的目的，让用户线程处理耗时较长的操作，避免阻塞tomcat的工作线程
- UI程序中，开线程进行其他操作，避免阻塞UI线程

<br>

#### 应用之提高效率

充分利用多核CPU的优势，提高运行效率，想象下面的场景，执行3个计算，最后将计算结果汇总

```bash
计算 1 花费 10 ms

计算 2 花费 11 ms

计算 3 花费 9 ms

汇总需要 1 ms
```

- 如果是串行执行，那么总共花费的时间是`10 + 11 + 9 + 1 = 31ms`
- 但如果是四核CPU，各个核心分别使用线程1执行计算1，线程2执行计算2，线程3执行计算3，那么3个线程是并行的，花费时间只取决于最长的那个线程运行的时间，即`11ms`最后加上汇总时间只会花费`12ms`

>**注意**
>
>需要在多核CPU才能提高效率，单核仍然是轮流执行

<br>

**结论**

1. 单核CPU下，多线程不能实际提高程序运行效率，只是为了能够在不同的任务之间切换，不同线程轮流使用CPU，不至于一个线程总占用CPU，别的线程没法干活
2. 多核CPU可以并行跑多个线程，但能否提高程序运行效率还是要分情况的
    - 有些任务，经过精心设计，将任务拆分，并行执行，当然可以提高程序的运行效率，但不是所有计算任务都能拆分（参考[阿姆达尔定律](https://baike.baidu.com/item/阿姆达尔定律?fromModule=lemma_search-box)）
    - 也不是所有任务都需要拆分，任务的目的如果不同，谈拆分和效率没啥意义
3. IO操作不占用CPU，只是我们一般拷贝文件使用的是`阻塞IO`，这时相当于线程虽然不用CPU，但需要一直等待IO结束，没能充分利用线程。所以才用后面的`非阻塞IO`和`异步IO`优化

<br>

## Java线程

### 创建和运行线程

#### 方法一 **Thread**

{{< admonition example Thread使用步骤>}}

1. 创建一个继承于Thread类的子类

2. 重写Thread类的run() ——> 将此线程执行的操作声明在run()中
3. 创建Thread类的子类的对象
4. 通过此对象调用start()执行线程

{{< /admonition >}}

**创建模版**

```java
// 创建线程对象 
Thread t = new Thread() {
	public void run() {
	// 要执行的任务
	} 
}; 
// 启动线程 
t.start();
```

**实际应用示例**

```java
// 构造方法的参数是给线程指定名字，推荐 
Thread t1 = new Thread("t1") {
	@Override 
  // run 方法内实现了要执行的任务
  public void run() { 
    log.debug("hello"); 
  }
}; t1.start();
```

**输出**

```bash
19:19:00 [t1] c.ThreadStarter - hello
```

<br>

#### 方法二 **Runable**

{{< admonition example Runable使用步骤>}}

1. 创建一个实现了Runnable接口的类
2. 实现类去实现Runnable中的抽象方法：run()
3. 创建实现类的对象
4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
5. 通过Thread类的对象调用start()
    - 启动线程
    - 调用当前线程的run()——>调用了Runnable类型的target的run()

{{< /admonition >}}

把线程和任务（需要执行的代码）分开

- Thread 代表线程
- Runable 可运行的任务（线程需要执行的代码）

**创建模版**

```java
Runnable runnable = new Runnable() { 
  public void run(){ 
    // 要执行的任务 
  } 
}; 
// 创建线程对象 
Thread t = new Thread( runnable ); 
// 启动线程 
t.start();
```

**实际应用示例**

```java
// 创建任务对象 
Runnable task2 = new Runnable() { 
  @Override public void run() { 
    log.debug("hello"); 
  } 
};

// 参数1 是任务对象; 参数2 是线程名字，推荐 
Thread t2 = new Thread(task2, "t2"); 
t2.start();
```

**输出**

```java
19:19:00 [t2] c.ThreadStarter - hello
```

<br>

Java 8以后可以使用 Lambda 精简代码

```java
// 创建任务对象 
Runnable task2 = () -> log.debug("hello");

// 参数1 是任务对象; 参数2 是线程名字，推荐 
Thread t2 = new Thread(task2, "t2"); 
t2.start();
```

<br>

**Thread 与 Runnable 的关系**

- 方法一是把线程和任务合并在了一起，方法二是把线程和任务分开了
- 用 Runable 更容易与线程池等高级 API 配合
- 用 Runable 让任务类脱离了 Thread 继承体系，更灵活

<br>

#### 方法三 **Callable**

{{< admonition example FutureTask使用步骤>}}

1. 创建一个实现 Callable 的实现类
2. 实现 call() 方法，将此线程需要执行的操作声明在 call() 中
3. 创建 Callable 接口实现类的对象
4. 将此 Callable 接口实现类的对象作为传递到 FutureTask 构造器中，创建 FutureTask 的对象
5. 将 FutureTask 的对象作为参数传递到 Thread 类的构造器中，创建 Thread 对象，并调用 start()
6. 获取 Callable 中 call() 方法的返回值

{{< /admonition >}}

**实现Callable接口的方式创建线程的强大之处**

- call() 可以有返回值的
- call() 可以抛出异常，被外面的操作捕获，获取异常的信息
- Callable 是支持泛型的

**实际应用示例**

```java
// 创建任务对象
FutureTask<Integer> task3 = new FutureTask<>(() -> {
	log.debug("hello");
  return 100;
});

// 参数1 是任务对象; 参数2 是线程名字，推荐 
new Thread(task3, "t3").start();

// 主线程阻塞，同步等待 task 执行完毕的结果 
Integer result = task3.get(); 
log.debug("结果是:{}", result);
```

**输出**

```bash
19:22:27 [t3] c.ThreadStarter - hello 
19:22:27 [main] c.ThreadStarter - 结果是:100
```

<br>

### 观察多个线程同时运行

**规律**

- 交替执行
- 谁先谁后，不由我们控制

<br>

### 查看进程线程运行的方法

#### **Windows**

- 任务管理器可以查看进程和线程数，也可以用来杀死进程
- `tasklist` 查看进程
- `taskkill` 杀死进程

<br>

#### **Linux**

- `ps -ef` 查看所有进程
- `ps -fT -p<PID>` 查看某个进程（PID）的所有线程
- `kill` 杀死进程
- `top` 按大写H切换是否显示线程
- `top -H -p<PID>` 查看某个进程（PID）的所有线程

<br>

#### **Java**

- `jps` 查看所有Java进程
- `jstack <PID>` 查看某个Java进程（PID）的所有线程状态
- `jconsole` 来查看某个Java进程中线程的运行状态（图形界面）

<br>

**jconsole** 远程监控配置

- 需要以如下方式运行Java类

```bash
java -Djava.rmi.server.hostname=`ip地址` -Dcom.sun.management.jmxremote 
Dcom.sun.management.jmxremote.port=`连接端口` -Dcom.sun.management.jmxremote.ssl=是否安全连接 
Dcom.sun.management.jmxremote.authenticate=是否认证 java类
```

- 修改 /etc/hosts 文件将 127.0.0.1 映射至主机名

**如果要认证访问，还需要做如下步骤**

- 复制 jmxremote.password 文件
- 修改 jmxremote.password 和 jmxremote.access 文件的权限为600即文件所有者可读写
- 连接时填入 controlRole（用户名），R&D（密码）

<br>

### 线程运行原理

#### 栈与栈帧

Java Virtual Machine Stacks（Java虚拟机栈）

我们都知道jVM中由堆、栈、方法区所组成，其中栈内存是给谁用的呢？其实就是线程，每个线程启动后，虚拟机就会为其分配一块栈内存

- 每个栈由多个栈帧（Frame）组成，对应着每次方法调用时所占用的内存
- 每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法

<br>

#### 线程上下文切换（Thread Context Switch）

因为以下一些原因导致CPU不再执行当前的线程，转而执行另一个线程的代码

- 线程的CPU时间片用完
- 垃圾回收
- 有更高优先级的线程需要运行
- 线程自己调用了 **sleep、yield、wait、join、park、synchronized、lock** 等方法

当 Context Switch 发生时，需要由操作系统保存当前线程的状态，并恢复另一个线程的状态，Java中对应的概念就是程序计数器（**Program Counter Register**），它的作用是记住下一条 JVM 指令的执行地址，是线程私有的

- 状态包括程序计数器、虚拟机栈中每个栈帧的信息，如局部变量、操作数栈、放回地址等
- **Context Switch** 频繁发生会影响性能

<br>

### 线程常见方法

|      方法名      | static | 功能说明                                                     | 注意                                                         |
| :--------------: | :----: | :----------------------------------------------------------- | :----------------------------------------------------------- |
|     start()      |        | 启动一个新线 程，在新的线程 运行 run 方法 中的代码           | start 方法只是让线程进入就绪，里面代码不一定立刻 运行（CPU 的时间片还没分给它）。每个线程对象的 start方法只能调用一次，如果调用了多次会出现 IllegalThreadStateException |
|      run()       |        | 新线程启动后会 调用的方法                                    | 如果在构造 Thread 对象时传递了 Runnable 参数，则 线程启动后会调用 Runnable 中的 run 方法，否则默 认不执行任何操作。但可以创建 Thread 的子类对象， 来覆盖默认行为 |
|      join()      |        | 等待线程运行结 束                                            |                                                              |
|   join(long n)   |        | 等待线程运行结 束,最多等待 n 毫秒                            |                                                              |
|     getId()      |        | 获取线程长整型 的 id                                         | id 唯一                                                      |
|    getName()     |        | 获取线程名                                                   |                                                              |
| setName(String)  |        | 修改线程名                                                   |                                                              |
|  getPriority()   |        | 获取线程优先级                                               |                                                              |
| setPriority(int) |        | 修改线程优先级                                               | java中规定线程优先级是1~10 的整数，较大的优先级 能提高该线程被 CPU 调度的机率 |
|    getState()    |        | 获取线程状态                                                 | Java 中线程状态是用 6 个 enum 表示，分别为： NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED |
| isInterrupted()  |        | 判断是否被打 断，                                            | 不会清除打断标记                                             |
|    isAlive()     |        | 线程是否存活 （还没有运行完 毕）                             |                                                              |
|   interrupt()    |        | 打断线程                                                     | 如果被打断线程正在 sleep，wait，join 会导致被打断 的线程抛出 InterruptedException，并清除 打断标 记 ；如果打断的正在运行的线程，则会设置 打断标 记 ；park 的线程被打断，也会设置 打断标记 |
|  interrupted()   | static | 判断当前线程是 否被打断                                      | 会清除打断标记                                               |
| currentThread()  | static | 获取当前正在执 行的线程                                      |                                                              |
|  sleep(long n)   | static | 让当前执行的线 程休眠n毫秒， 休眠时让出 cpu 的时间片给其它 线程 |                                                              |
|     yield()      | static | 提示线程调度器 让出当前线程对 CPU的使用                      | 主要是为了测试和调试                                         |

<br>

### start 与 run

#### 调用 run()

```java
public static void main(String[] args) {
  Thread t1 = new Thread("t1") { 
    @Override public void run() { 
      log.debug(Thread.currentThread().getName());
      FileReader.read(Constants.MP4_FULL_PATH); 
    }
};
  
  t1.run(); 
  log.debug("do other things ...");
}
```

**输出**

```bash
19:39:14 [main] c.TestStart - main 
19:39:14 [main] c.FileReader - read [1.mp4] start ...
19:39:18 [main] c.FileReader - read [1.mp4] end ... cost: 4227 ms 
19:39:18 [main] c.TestStart - do other things ...
```

程序仍在main线程运行，`FileReader.read()`方法调用还是同步的

<br>

#### 调用 start()

将上述代码的`t1.run()`改为

```java
t1.start();
```

**输出**

```bash
19:41:30 [main] c.TestStart - do other things ...
19:41:30 [t1] c.TestStart - t1 
19:41:30 [t1] c.FileReader - read [1.mp4] start ...
19:41:35 [t1] c.FileReader - read [1.mp4] end ... cost: 4542 ms
```

程序在t1线程中运行，`FileReader.read()`方法调用是异步的

<br>

#### 小结

- 直接调用 run() 是在主线程中执行了run方法中内容，没有真正启动新的线程
- 使用 start() 是启动新的线程，通过新的线程间接执行了run方法中的代码

<br>

### sleep 与 yield

#### sleep

1. 调用 sleep 会让当前线程从 Running 进入 Timed Waiting 状态（阻塞）
2. 其他线程可以使用 interrupt 方法打断正在睡眠的线程，这时 sleep 方法会跑出 `InterruptedException`
3. 睡眠结束后的线程未必会立刻得到执行
4. 建议使用 TimeUnit 的 sleep 代替 Thread 的 sleep 来获得更好的可读性

<br>

#### yield

1. 调用 yield 会让当前线程从 Running 进入 Runnable 就绪状态，然后调度执行其他线程
2. 具体的实现依赖于操作系统的任务调度器

<br>

#### 线程优先级

- 线程优先级会提示（hint）调度器优先调度该线程，但它仅仅是一个提示，调度器可以忽略它
- 如果 CPU 比较忙，那么优先级高的线程会获得更多的时间片，但 CPU 空闲时，优先级几乎没有作用

```java
Runnable task1 = () -> { 
  int count = 0; 
  for (;;) { 
    System.out.println("---->1 " + count++); 
  } 
}; 

Runnable task2 = () -> { 
  int count = 0; 
  for (;;) { 
    // Thread.yield(); 
    System.out.println("---->2 " + count++);
  }
};

Thread t1 = new Thread(task1, "t1");
Thread t2 = new Thread(task2, "t2"); 
// t1.setPriority(Thread.MIN_PRIORITY);
// t2.setPriority(Thread.MAX_PRIORITY); 
t1.start(); 
t2.start();
```

<br>

### join方法详解

#### 为什么需要join

执行下面代码，打印r是什么？

```java
static int r = 0; 
public static void main(String[] args) throws InterruptedException { 
  test1(); 
} 
private static void test1() throws InterruptedException { 
  log.debug("开始"); 
  Thread t1 = new Thread(() -> { 
    log.debug("开始"); 
    sleep(1); 
    log.debug("结束");
    r = 10;
  }); 
  t1.start(); 
  log.debug("结果为:{}", r); 
  log.debug("结束");
}
```

**分析**

- 因为主线程和线程t1是并行执行的，t1线程需要1秒之后才能算出`r=10`
- 而主线程一开始就要打印r的结果，所以只能打印出`r=0`

**解决办法**

- 使用 sleep 行不行？为什么？
- 用 join，加在`t1.start()`之后即可

<br>

#### 同步应用案例

以调用方角度来讲，如果

- 需要等待结果返回，才能继续运行就是同步
- 不需要等待结果返回，就能继续运行就是异步

![同步](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%90%8C%E6%AD%A5.jpg "同步")

<br>

**等待多个结果**

问，下面代码 cost 大约多少秒？

```java
static int r1 = 0; 
static int r2 = 0; 
public static void main(String[] args) throws InterruptedException {
  test2();
}
private static void test2() throws InterruptedException {
Thread t1 = new Thread(() -> {
  sleep(1);
  r1 = 10;
}); 
  Thread t2 = new Thread(() -> { 
    sleep(2);
    r2 = 20;
  }); 
  long start = System.currentTimeMillis();
  t1.start(); 
  t2.start(); 
  t1.join(); 
  t2.join(); 
  long end = System.currentTimeMillis();
  log.debug("r1: {} r2: {} cost: {}", r1, r2, end - start);
}
```

**分析如下**

- 第一个 join：等待t1时，t2并没有停止，而在运行
- 第二个 join：1s后，执行到此，t2也运行了1s，因此也只需再等待1s

如果颠倒两个 join 呢？

```bash
20:45:43.239 [main] c.TestJoin - r1: 10 r2: 20 cost: 2005
```

![异步](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%BC%82%E6%AD%A5.jpg "异步")

<br>

#### 有时效的join

**等待时间**

```java
static int r1 = 0; 
static int r2 = 0; 
public static void main(String[] args) throws InterruptedException {
  test3();
}
public static void test3() throws InterruptedException {
  Thread t1 = new Thread(() -> {
    sleep(1);
    r1 = 10;
  });
  
  long start = System.currentTimeMillis();
  
  t1.start();
  // 线程执行结束会导致 join 结束 
  t1.join(1500); 
  long end = System.currentTimeMillis();
  log.debug("r1: {} r2: {} cost: {}", r1, r2, end - start);
}
```

**输出**

```bash
20:48:01.320 [main] c.TestJoin - r1: 10 r2: 0 cost: 1010
```

**没等够时间**

```java
static int r1 = 0; 
static int r2 = 0; 
public static void main(String[] args) throws InterruptedException {
  test3(); 
} 
public static void test3() throws InterruptedException {
  Thread t1 = new Thread(() -> {
    sleep(2);
    r1 = 10;
  });
  long start = System.currentTimeMillis();
  t1.start();
  // 线程执行结束会导致 join 结束 
  t1.join(1500); 
  long end = System.currentTimeMillis();
  log.debug("r1: {} r2: {} cost: {}", r1, r2, end - start);
}
```

**输出**

```bash
20:52:15.623 [main] c.TestJoin - r1: 0 r2: 0 cost: 1502
```

<br>

### interrupt方法详解

#### 打断 sleep，wait，join 的线程

**sleep，wait，join **这些方法都会让线程进入阻塞状态

打断 sleep 的线程，会清空打断状态，以sleep为例

```java
private static void test1() throws InterruptedException { 
  Thread t1 = new Thread(()->{ 
    sleep(1); 
  }, "t1"); 
  t1.start();
  sleep(0.5); 
  t1.interrupt(); 
  log.debug(" 打断状态: {}",t1.isInterrupted());
}
```

**输出**

```bash
java.lang.InterruptedException: sleep interrupted 
  at java.lang.Thread.sleep(Native Method) 
  at java.lang.Thread.sleep(Thread.java:340)
  at java.util.concurrent.TimeUnit.sleep(TimeUnit.java:386) 
  at cn.itcast.n2.util.Sleeper.sleep(Sleeper.java:8) 
  at cn.itcast.n4.TestInterrupt.lambda$test1$3(TestInterrupt.java:59) 
  at java.lang.Thread.run(Thread.java:745) 
  21:18:10.374 [main] c.TestInterrupt - 打断状态: false
```

#### 打断正常运行的线程

打断正常运行的线程，不会清空打断状态

```java
private static void test2() throws InterruptedException {
  Thread t2 = new Thread(()->{
    while(true) {
      Thread current = Thread.currentThread(); 
      boolean interrupted = current.isInterrupted(); 
      if(interrupted) {
        log.debug(" 打断状态: {}", interrupted);
        break; 
      }
    } 
  }, "t2"); 
  t2.start();
  sleep(0.5); 
  t2.interrupt();
}
```

**输出**

```bash
20:57:37.964 [t2] c.TestInterrupt - 打断状态: true
```

#### 打断 park 线程

打断 park 线程，不会清空打断状态

```java
private static void test3() throws InterruptedException {
  Thread t1 = new Thread(() -> {
    log.debug("park..."); LockSupport.park();
    log.debug("unpark..."); log.debug("打断状态：{}",Thread.currentThread().isInterrupted()); 
  }, "t1"); 
  t1.start();
  
  sleep(0.5);
  t1.interrupt();
}
```

**输出**

```bash
21:11:52.795 [t1] c.TestInterrupt - park... 
21:11:53.295 [t1] c.TestInterrupt - unpark... 
21:11:53.295 [t1] c.TestInterrupt - 打断状态：true
```

<br>

如果打断标记已经是 true，则 park 会失效

```java
private static void test4() { 
  Thread t1 = new Thread(() -> {
    for (int i = 0; i < 5; i++) {
      log.debug("park...");
      LockSupport.park(); 
      log.debug("打断状态：{}", Thread.currentThread().isInterrupted());
    }
  }); 
  t1.start();
  sleep(1); 
  t1.interrupt();
}
```

输出

```bash
21:13:48.783 [Thread-0] c.TestInterrupt - park... 
21:13:49.809 [Thread-0] c.TestInterrupt - 打断状态：true 
21:13:49.812 [Thread-0] c.TestInterrupt - park... 
21:13:49.813 [Thread-0] c.TestInterrupt - 打断状态：true 
21:13:49.813 [Thread-0] c.TestInterrupt - park... 
21:13:49.813 [Thread-0] c.TestInterrupt - 打断状态：true 
21:13:49.813 [Thread-0] c.TestInterrupt - park... 
21:13:49.813 [Thread-0] c.TestInterrupt - 打断状态：true 
21:13:49.813 [Thread-0] c.TestInterrupt - park... 
21:13:49.813 [Thread-0] c.TestInterrupt - 打断状态：true
```

> 提示
>
> 可以使用`Thread.interrupted()`	清除打断状态

<br>

### 不推荐的方法

还有一些不推荐使用的方法，这些方法已过时，容易破坏同步代码块，造成线程死锁

|  方法名   | static |       功能说明       |
| :-------: | :----: | :------------------: |
|  stop()   |        |     停止线程运行     |
| suspend() |        | 挂起（暂停）线程运行 |
| resume()  |        |     恢复线程运行     |

<br>

### 主线程与守护线程

默认情况下，Java 进程需要等待所有线程都运行结束，才会结束。有一种特殊的线程叫做**守护线程**，只要其他非守护线程运行结束了，即使守护线程的代码没有执行完，也会强制结束。

**应用示例**

```java
log.debug("开始运行..."); 
Thread t1 = new Thread(() -> {
  log.debug("开始运行...");
  sleep(2);
  log.debug("运行结束..."); 
}, "daemon"); 
// 设置该线程为守护线程 t1.setDaemon(true);
t1.start();

sleep(1);
log.debug("运行结束...");
```

**输出**

```bash
08:26:38.123 [main] c.TestDaemon - 开始运行... 
08:26:38.213 [daemon] c.TestDaemon - 开始运行... 
08:26:39.215 [main] c.TestDaemon - 运行结束...
```

>**注意**
>
>- 垃圾回收器线程就是一种守护线程
>- Tomcat 中的 Acceptor 和 Poller 线程都是守护线程，所以 Tomcat 接收到 shutdown 命令后，不会等待它们处理完当前请求

<br>

### 五种状态

这是从 **操作系统** 层面来划分描述的

![线程五种状态](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E7%BA%BF%E7%A8%8B%E4%BA%94%E7%A7%8D%E7%8A%B6%E6%80%81.jpg "线程五种状态")

- 【**初始状态**】仅是在语言层面创建了线程对象，还未与操作系统线程关联 
- 【**可运行状态**】（就绪状态）指该线程已经被创建（与操作系统线程关联），可以由 CPU 调度执行 
- 【**运行状态**】指获取了 CPU 时间片运行中的状态 
    1. 当 CPU 时间片用完，会从【**运行状态**】转换至【**可运行状态**】，会导致线程的上下文切换 
- 【**阻塞状态**】 
    1. 如果调用了阻塞 API，如 BIO 读写文件，这时该线程实际不会用到 CPU，会导致线程上下文切换，进入 【**阻塞状态**】 
    2. 等 BIO 操作完毕，会由操作系统唤醒阻塞的线程，转换至【**可运行状态**】 
    3. 与【**可运行状态**】的区别是，对【**阻塞状态**】的线程来说只要它们一直不唤醒，调度器就一直不会考虑 调度它们 
- 【**终止状态**】表示线程已经执行完毕，生命周期已经结束，不会再转换为其它状态

<br>

### 六种状态

这时从 **Java API** 层面来描述的

根据 Thread.State 枚举，分为六种状态

![线程六种状态](https://cdn.jsdelivr.net/gh/Turbo-King/images/线程六种状态.jpg "线程六种状态")

- `NEW` 线程刚被创建，但是还没有调用
- `RUNNABLE` 当调用了 `start()` 方法 方法之后，注意，**Java API** 层面的 `RUNNABLE` 状态涵盖了 **操作系统** 层面的【**可运行状态**】、【**运行状态**】和【**阻塞状态**】（由于 BIO 导致的线程阻塞，在 Java 里无法区分，仍然认为 是可运行）
-  `BLOCKED` ， `WAITING` ， `TIMED_WAITING` 都是 **Java API** 层面对【**阻塞状态**】的细
- `TERMINATED` 当线程代码运行结束

<br>

## 共享模型

### Java体现

两个线程对初始值为0的静态变量一个做自增，一个做自减，各做5000次，结果是0吗？

```java
static int counter = 0; 
public static void main(String[] args) throws InterruptedException { 
  Thread t1 = new Thread(() -> { 
    for (int i = 0; i < 5000; i++) {
      counter++; 
    } 
  }, "t1"); 
  Thread t2 = new Thread(() -> {
    for (int i = 0; i < 5000; i++) {
      counter--; 
    } 
  }, "t2"); 
  t1.start(); 
  t2.start(); 
  t1.join();
  t2.join(); 
  log.debug("{}",counter);
}
```

**问题分析**

以上的结果可能是正数、负数、零。为什么呢？因为Java中对静态变量的自增，自减并不是原子操作，要彻底理解，必须从字节码角度来进行分析

例如对于 `i++` 而言（i为静态变量），实际会产生如下的JVM字节码指令：

```sh
getstatic  i  // 获取静态变量i的值
iconst_1      // 准备常量1
iadd          // 自增 
putstatic  i  // 将修改后的值存入静态变量i
```



而对应 `i--` 也是类似：

```sh
getstatic  i  // 获取静态变量 i 的值
iconst_1      // 准备常量1
isub          // 自减 
putstatic  i  // 将修改后的值存入静态变量 i
```

而Java的内存模型如下，完成静态变量的自增，自减需要在主存和工作内存中进行数据交换

![Java内存模型](https://cdn.jsdelivr.net/gh/Turbo-King/images/Java%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B.jpg "Java内存模型")

如果是单线程以上8行代码是顺序执行（不会交错）没有问题：

![单线程下运行](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%8D%95%E7%BA%BF%E7%A8%8B%E4%B8%8B%E8%BF%90%E8%A1%8C.jpg "单线程下运行")

但多线程下这8行代码可能交错运行：

出现负数的情况：

![多线程下运行下出现负数情况](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%A4%9A%E7%BA%BF%E7%A8%8B%E4%B8%8B%E8%BF%90%E8%A1%8C.jpg "多线程下运行下出现负数情况")

出现正数的情况：

![多线程下运行出现正数情况](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%A4%9A%E7%BA%BF%E7%A8%8B%E4%B8%8B%E8%BF%90%E8%A1%8C%E5%87%BA%E7%8E%B0%E6%AD%A3%E6%95%B0%E6%83%85%E5%86%B5.jpg "多线程下运行出现正数情况")

<br>

#### 临界区Critical Section

- 一个程序运行多个线程本身是没有问题的
- 问题出在多线程访问**共享资源**
    1. 多个线程读**共享资源**其实也没有问题
    2. 在多个线程对**共享资源**读写操作时发生指令交错，就会出现问题
- 一段代码块如果存在对**共享资源**的多线程读写操作，称这段代码块为**临界区**

例如，下面代码中的临界区

```java
static int counter = 0;

static void increment() 
  // 临界区
{ 
  counter++;
}

static void decrement() 
  // 临界区 
{ 
  counter--; 
}
```

<br>

#### 竞态条件Race Condition

多个线程在临界区内执行，由于代码的**执行序列不同**而导致结果无法预测，称之为发生了**竞态条件**

<br>

### Synchronized

为了避免临界区的竞态条件发生，由多种手段可以达到目的

- 阻塞式的解决方案：**Synchronized**，**Lock**
- 非阻塞式的解决方案：原子变量

本次使用阻塞式的解决方案：**Synchronized**，来解决上述问题，即俗称的 **对象锁** ，它采用互斥的方式让同一时刻至多只有一个线程能持有 **对象锁** ，其他线程再想获取这个 **对象锁** 时就会被阻塞住，这样就能保证拥有锁的线程可以安全的执行临界区的代码，不用担心线程上下文切换

>**注意**
>
>虽然Java中互斥和同步都可以采用Synchronized关键字来完成，但它们还是有区别的
>
>- 互斥是保证临界区的竞态发生，同一时刻只能有一个线程执行临界区代码
>- 同步是由于线程执行的先后，顺序不同，需要一个线程等待其他线程运行到某个点

**Synchronized语法**

```java
synchronized(对象) // 线程1， 线程2(blocked) 
{ 
  临界区 
}
```

**解决**

```java
static int counter = 0; 
static final Object room = new Object(); 

public static void main(String[] args) throws InterruptedException { 
  Thread t1 = new Thread(() -> { 
    for (int i = 0; i < 5000; i++) {
      synchronized (room) { 
        counter++; 
      } 
    } 
  }, "t1"); 
  
  Thread t2 = new Thread(() -> { 
    synchronized (room) { 
      for (int i = 0; i < 5000; i++) {
        counter--; 
      } 
    } 
  }, "t2"); 
  
  t1.start(); 
  t2.start(); 
  t1.join(); 
  t2.join(); 
  log.debug("{}",counter);
}
```

**执行过程**

![Synchronized执行过程](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%AD%A3%E7%A1%AE%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B.jpg "Synchronized执行过程")

<br>

#### 面向对象改进

把需要保护的共享变量放入一个类

```java
class Room { 
  int value = 0;
  
  public void increment() { 
    synchronized (this) { 
      value++; 
    } 
  }
  
  public void decrement() { 
    synchronized (this) { 
      value--; 
    } 
  }
  
  public int get() { 
    synchronized (this) { 
      return value; 
    } 
  }
}


@Slf4j public class Test1 {
  public static void main(String[] args) throws InterruptedException { 
    Room room = new Room(); 
    
    Thread t1 = new Thread(() -> {
      for (int j = 0; j < 5000; j++) {
        room.increment();
      } 
    }, "t1");
    
    Thread t2 = new Thread(() -> { 
      for (int j = 0; j < 5000; j++) {
        room.decrement(); 
      } 
    }, "t2"); 
    
    t1.start(); 
    t2.start();
    
    t1.join(); 
    t2.join(); 
    log.debug("count: {}" , room.get());
  }
}
```

<br>

#### 方法上的synchronized

```java
class Test{ 
  public synchronized void test() {
    
  }
} 

等价于 class Test{
  public void test() { 
    synchronized(this) {
      
    }
  }
}




class Test{ 
  public synchronized static void test() {
    
  }
} 


等价于 
class Test{
  public static void test() { 
    synchronized(Test.class) {
      
    }
  }
}
```

<br>

#### 不加synchronzied的方法

**不加 synchronized 的方法就好比不遵守规则的人，不去老实排队（好比翻窗户进去的）**

<br>

### 线程八锁

其实就是考察synchronized锁住的是哪个对象

**情况1：12 或 21**

```java
@Slf4j(topic = "c.Number") 
class Number{
  public synchronized void a() {
    log.debug("1"); 
  } 
  public synchronized void b() {
    log.debug("2"); 
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  new Thread(()->{ n1.a(); }).start();
  new Thread(()->{ n1.b(); }).start(); 
}
```

<br>

**情况2：1s后 12，或2 1s后 1**

```java
@Slf4j(topic = "c.Number") 
class Number{
  public synchronized void a() { 
    sleep(1); 
    log.debug("1"); 
  } 
  public synchronized void b() {
    log.debug("2"); 
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  new Thread(()->{ n1.a(); }).start(); 
  new Thread(()->{ n1.b(); }).start(); 
}
```

<br>

**情况3：3 1s后 12，或23 1s后 1，或32 1s后 1**

```java
@Slf4j(topic = "c.Number")
class Number{
  public synchronized void a() { 
    sleep(1); 
    log.debug("1"); 
  } 
  public synchronized void b() {
    log.debug("2"); 
  } 
  public void c() {
    log.debug("3"); 
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  new Thread(()->{ n1.a(); }).start();
  new Thread(()->{ n1.b(); }).start();
  new Thread(()->{ n1.c(); }).start(); 
}
```

<br>

**情况4：2 1s后 1**

```java
@Slf4j(topic = "c.Number") 
class Number{
  public synchronized void a() { 
    sleep(1); 
    log.debug("1"); 
  } 
  public synchronized void b() {
    log.debug("2"); 
  }
}

public static void main(String[] args) {
  Number n1 = new Number();
  Number n2 = new Number(); 
  new Thread(()->{ n1.a(); }).start(); 
  new Thread(()->{ n2.b(); }).start(); 
}
```

<br>

**情况5：2 1s后 1**

```java
@Slf4j(topic = "c.Number") 
class Number{
  public static synchronized void a() {
    sleep(1); 
    log.debug("1"); 
  } 
  
  public synchronized void b() {
    log.debug("2");
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  new Thread(()->{ n1.a(); }).start(); 
  new Thread(()->{ n1.b(); }).start(); 
}
```

<br>

**情况6：1s后 12，或2 1s后 1**

```java
@Slf4j(topic = "c.Number") 
class Number{
  public static synchronized void a() {
    sleep(1); 
    log.debug("1"); 
  } 
  
  public static synchronized void b() {
    log.debug("2"); 
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  new Thread(()->{ n1.a(); }).start();
  new Thread(()->{ n1.b(); }).start(); 
}
```

<br>

**情况7：2 1s后 1**

```java
@Slf4j(topic = "c.Number") 
class Number{
  public static synchronized void a() {
    sleep(1); 
    log.debug("1"); 
  } 
  
  public synchronized void b() {
    log.debug("2");
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  Number n2 = new Number(); 
  new Thread(()->{ n1.a(); }).start(); 
  new Thread(()->{ n2.b(); }).start(); 
}
```

<br>

**情况8：1s后 12，或2 1s后 1**

```java
@Slf4j(topic = "c.Number")
class Number{
  public static synchronized void a() {
    sleep(1); 
    log.debug("1"); 
  } 
  
  public static synchronized void b() {
    log.debug("2"); 
  }
}

public static void main(String[] args) {
  Number n1 = new Number(); 
  Number n2 = new Number(); 
  new Thread(()->{ n1.a(); }).start(); 
  new Thread(()->{ n2.b(); }).start(); 
}
```

<br>

#### 变量的线程安全分析

1. **成员变量和静态变量是否线程安全？**
    - 如果它们没有共享，则线程安全
    - 如果它们被共享了，根据它们的状态是否能够改变，又分两种情况
        - 如果只有读操作，则线程安全
        - 如果有读写操作，则这段代码是临界区，需要考虑线程安全
2. **局部变量是否线程安全？**
    - 局部变量是线程安全的
    - 但局部变量引用的对象则未必
        - 如果该对象没有逃离方法的作用访问，它是线程安全的
        - 如果该对象逃离方法的作用范围，需要考虑线程安全
3. **局部变量线程安全分析？**

```java
public static void test1() {
  int i = 10;
  i++;
}
```

每个线程调用test1()方法时局部变量i，会在每个线程的栈帧内存中被创建多份，因此不存在共享

```asciiarmor
public static void test1();
	descriptor: ()V
  flags: ACC_PUBLIC, ACC_STATIC 
  Code:
		stack=1, locals=1, args_size=0 
      0: bipush 10 
      2: istore_0 
      3: iinc 0, 1 
      6: return 
    LineNumberTable:
			line 10: 0 
      line 11: 3 
      line 12: 6 
    LocalVariableTable:
			Start Length Slot Name Signature 
        3      4     0    i      I
```

如下图展示

![栈帧展示](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%A0%88%E5%B8%A7%E5%B1%95%E7%A4%BA.jpg "栈帧展示")

局部变量的引用稍有不同

先看一个成员变量的例子

```java
class ThreadUnsafe {
  ArrayList<String> list = new ArrayList<>();
  public void method1(int loopNumber) {
    for (int i = 0; i < loopNumber; i++) {
      // { 临界区, 会产生竞态条件 method2();
      method3();
      // } 临界区
    }
  }
  
  private void method2() { 
    list.add("1"); 
  }
  
  private void method3() { 
    list.remove(0);
  }
}
```

执行
```java
static final int THREAD_NUMBER = 2; 
static final int LOOP_NUMBER = 200; 
public static void main(String[] args) {
  ThreadUnsafe test = new ThreadUnsafe();
  for (int i = 0; i < THREAD_NUMBER; i++) {
    new Thread(() -> {
      test.method1(LOOP_NUMBER); 
    }, "Thread" + i).start();
  }
}
```

其中一种情况是，如果线程2还未add，线程1 remove就会报错：

```shell
Exception in thread "Thread1" java.lang.IndexOutOfBoundsException: Index: 0, Size: 0 
  at java.util.ArrayList.rangeCheck(ArrayList.java:657) 
  at java.util.ArrayList.remove(ArrayList.java:496) 
  at cn.itcast.n6.ThreadUnsafe.method3(TestThreadSafe.java:35) 
  at cn.itcast.n6.ThreadUnsafe.method1(TestThreadSafe.java:26) 
  at cn.itcast.n6.TestThreadSafe.lambda$main$0(TestThreadSafe.java:14) 
  at java.lang.Thread.run(Thread.java:748)
```

分析：

- 无论哪个线程中的 method2 引用的都是同一个对象中的 list 成员变量
- method3 与 method2 分析相同

![栈帧分析](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%A0%88%E5%B8%A7%E5%88%86%E6%9E%90.jpg "栈帧分析")

将 list 修改为局部变量，就不会发生上述问题

```java
class ThreadSafe { 
  public final void method1(int loopNumber) {
    ArrayList<String> list = new ArrayList<>();
    for (int i = 0; i < loopNumber; i++) {
      method2(list); 
      method3(list);
    }
  }
  
  private void method2(ArrayList<String> list) { 
    list.add("1"); 
  }
  
  private void method3(ArrayList<String> list) { 
    list.remove(0); 
  }
}
```

分析：

- list 是局部变量，每个线程调用时会创建其不同实例，没有共享
- 而 method2 的参数是从 method1 中传递过来的，与 method1 中引用同一个对象
- method3 的参数分析与 method2 相同

![list为局部变量栈帧分析](https://cdn.jsdelivr.net/gh/Turbo-King/images/list%E4%B8%BA%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F%E6%A0%88%E5%B8%A7%E5%88%86%E6%9E%90.jpg "list为局部变量栈帧分析")

方法访问修饰符带来的思考，如果把 method2 和 method3 的方法修改为public会不会代理线程安全问题？

- 情况1：有其他线程调用 method1 和 method3
- 情况2：在情况1的基础上，为 ThreadSafe 类添加子类，子类覆盖 method2 或 method3 方法，即

```java
class ThreadSafe { 
  public final void method1(int loopNumber) {
    ArrayList<String> list = new ArrayList<>();
    for (int i = 0; i < loopNumber; i++) {
      method2(list); method3(list);
    }
  }
  
  private void method2(ArrayList<String> list) {
    list.add("1");
  }
  
  private void method3(ArrayList<String> list) { 
    list.remove(0); 
  }
}

class ThreadSafeSubClass extends ThreadSafe{
  
  @Override 
  public void method3(ArrayList<String> list) {
    new Thread(() -> {
      list.remove(0);
    }).start(); 
  }
}
```

> 从这个例子可以看出 private 或 final 提供【安全】的意义所在，请体会开闭原则中的【闭】

<br>

### 常见线程安全类

- String
- Integer
- StringBuffer
- Random
- Vector
- Hashtable
- java.util.concurrent包下的类

这里说它们是线程安全的是指，多个线程调用它们同一个实例的某个方法时，是线程安全的。也可以理解为

```java
Hashtable table = new Hashtable();

new Thread(()->{ 
  table.put("key", "value1"); 
}).start();

new Thread(()->{ 
  table.put("key", "value2"); 
}).start();
```

> - 它们每个方法是原子的
> - 但注意它们多个方法的组合不是原子的

#### 线程安全类方法的组合

分析下面代码是否线程安全

```java
Hashtable table = new Hashtable(); 
// 线程1，线程2 
if( table.get("key") == null) {
  table.put("key", value); 
}
```

![代码图解](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BB%A3%E7%A0%81%E5%9B%BE%E8%A7%A3.jpg "代码图解")

<br>

#### 不可变类线程安全性

String、Integer等都是不可变类，因为其内部的状态不可以改变，因此它们的方法都是线程安全的。

```java
public class Immutable{ 
  private int value = 0;
  
  public Immutable(int value){ 
    this.value = value;
  }
  
  public int getValue(){ 
    return this.value; 
  }
}
```

如果想增加一个增加的方法呢

```java
public class Immutable{ 
  private int value = 0;
  
  public Immutable(int value){ 
    this.value = value; 
  }
  
  public int getValue(){
    return this.value; 
  }
  
  public Immutable add(int v){ 
    return new Immutable(this.value + v);
  }
}
```

<br>

### Monitor概念

#### Java对象头

以32位虚拟机为例

**普通对象**

![普通对象头](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%99%AE%E9%80%9A%E5%AF%B9%E8%B1%A1%E5%A4%B4.jpg "普通对象头")

<br>

**数组对象**

![数组对象头](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%95%B0%E7%BB%84%E5%AF%B9%E8%B1%A1%E5%A4%B4.jpg "数组对象头")

<br>

**其中 Mark Word 结构为**

![MarkWord结构](https://cdn.jsdelivr.net/gh/Turbo-King/images/MarkWord%E7%BB%93%E6%9E%84.jpg "MarkWord结构")

<br>

**64位虚拟机 Mark Word**

![64位虚拟机MarkWord](https://cdn.jsdelivr.net/gh/Turbo-King/images/64%E4%BD%8D%E8%99%9A%E6%8B%9F%E6%9C%BAMarkWord.jpg "64位虚拟机MarkWord")

> **[参考资料](https://stackoverflow.com/questions/26357186/what-is-in-java-object-header)**

<br>

### wait notify

#### API 介绍

- `obj.wait()`让进入object监视器的线程到waitSet等待
- `obj.notify()`在object上正在waitSet等待的线程中挑一个唤醒
- `obj.notifyAll()`让object上正在waitSet等待的线程全部唤醒

它们都是线程之间进行协作的手段，都属于Object对象的方法，必须获得此对象的锁，才能调用这几个方法

```java
final static Object obj = new Object();

public static void main(String[] args) {
  new Thread(() -> {
    synchronized (obj) { 
      log.debug("执行...."); 
      try { 
        obj.wait(); // 让线程在obj上一直等待下去 
      } catch (InterruptedException e) {
        e.printStackTrace(); 
      } 
      log.debug("其它代码....");
    } 
  }).start();
  
  new Thread(() -> {
    synchronized (obj) { 
      log.debug("执行...."); 
      try { obj.wait(); // 让线程在obj上一直等待下去 
          } catch (InterruptedException e) {
        e.printStackTrace(); 
      } 
      log.debug("其它代码....");
    } 
  }).start();
  
  // 主线程两秒后执行
  sleep(2); 
  log.debug("唤醒 obj 上其它线程");
  synchronized (obj) {
    obj.notify(); // 唤醒obj上一个线程
    // obj.notifyAll(); // 唤醒obj上所有等待线程 
  }
}
```

**notify的一种结果**

```shell
20:00:53.096 [Thread-0] c.TestWaitNotify - 执行.... 
20:00:53.099 [Thread-1] c.TestWaitNotify - 执行.... 
20:00:55.096 [main] c.TestWaitNotify - 唤醒 obj 上其它线程 
20:00:55.096 [Thread-0] c.TestWaitNotify - 其它代码....
```

**notifyAll结果**

```shell
19:58:15.457 [Thread-0] c.TestWaitNotify - 执行.... 
19:58:15.460 [Thread-1] c.TestWaitNotify - 执行.... 
19:58:17.456 [main] c.TestWaitNotify - 唤醒 obj 上其它线程 
19:58:17.456 [Thread-1] c.TestWaitNotify - 其它代码.... 
19:58:17.456 [Thread-0] c.TestWaitNotify - 其它代码....
```

`wait()`方法会释放对象的锁，进入WaitSet等待区，从而让其他线程就就有机会获取对象的锁。无限制等待，直到notify为止

`wait(long n)`有时限的等待，到n毫秒后结束等待，或是被notify

{{< admonition example "sleep(long n) 和 wait(long n) 的区别" >}}

1. **sleep 是 Thread 方法，而 wait 是 Object 的方法**
2. sleep **不需要强制**和 synchronized 配合使用，但 **wait 需要和 synchronized 一起使用**
3. **sleep 在睡眠的同时，不会释放对象锁，但 wait 在等待的时候会释放对象锁**
4. 它们状态 **TIMED_WAITINGsss**

{{< /admonition >}}

<br>

### Park & Unpark

#### 基本使用

它们是 LockSupport 类中的方法

```java
// 暂停当前线程 
LockSupport.park();

// 恢复某个线程的运行 
LockSupport.unpark(暂停线程对象)
```

**先 park 再 unpark**

```java
Thread t1 = new Thread(() -> {
  log.debug("start..."); sleep(1);
  log.debug("park...");
  LockSupport.park();
  log.debug("resume..."); 
},"t1");

t1.start();

sleep(2); 
log.debug("unpark..."); LockSupport.unpark(t1);
```

**输出结果**

```shell
18:42:52.585 c.TestParkUnpark [t1] - start... 
18:42:53.589 c.TestParkUnpark [t1] - park... 
18:42:54.583 c.TestParkUnpark [main] - unpark... 
18:42:54.583 c.TestParkUnpark [t1] - resume...
```

**先 unpark 再 park**

```java
Thread t1 = new Thread(() -> {
  log.debug("start..."); sleep(2);
  log.debug("park...");
  LockSupport.park();
  log.debug("resume..."); 
}, "t1"); 

t1.start();

sleep(1); 
log.debug("unpark..."); LockSupport.unpark(t1);
```

**输出结果**

```shell
18:43:50.765 c.TestParkUnpark [t1] - start... 
18:43:51.764 c.TestParkUnpark [main] - unpark... 
18:43:52.769 c.TestParkUnpark [t1] - park... 
18:43:52.769 c.TestParkUnpark [t1] - resume...
```

#### 特点

{{< admonition "Object 的 wait & notify 相比">}}

- wait，notify 和 notifyAll 必须配合 Object Monitor 一起使用，而 park，unpark不必
- park & unpark 是以线程为单位来【阻塞】和【唤醒】线程，而 notify 只能随机唤醒一个等待线程，notifyAll 是唤醒所有等待线程，就不那么【精确】
- park & unpark 可以先 unpark，而 wait & notify 不能先 notify

{{< /admonition >}}

<br>

### 重新理解线程状态转换

![线程状态转换图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2%E5%9B%BE.jpg "线程状态转换图")

假设有线程`Thread t`

#### 情况1 `NEW -> RUNABLE`

当调用`t.start()`方法时，由`NEW -> RUNNABLE`

#### 情况2 `RUNNABLE <-> WAITING`

t 线程调用`synchronized(obj)`获取了对象锁后

- 调用`obj.wait()`方法时，t 线程从`RUNNABLE -> WAITING`
- 调用`obj.notify()`,`obj.notifyAll()`,`t.interrupt`时
    - 竞争锁成功，t 线程从`WAITING -> RUNNABLE`
    - 竞争锁失败，t 线程从`WAITING -> BLOCKED`

```java
public class TestWaitNotify { 
  final static Object obj = new Object();
  
  public static void main(String[] args) {
    new Thread(() -> {
      synchronized (obj) { 
        log.debug("执行...."); 
        try { 
          obj.wait(); 
        } catch (InterruptedException e) { 
          e.printStackTrace(); 
        } 
        log.debug("其它代码...."); // 断点
      } 
    },"t1").start();
    
    new Thread(() -> {
      synchronized (obj) { 
        log.debug("执行...."); 
        try { 
          obj.wait(); 
        } catch (InterruptedException e) { 
          e.printStackTrace(); 
        } 
        log.debug("其它代码...."); // 断点
      } 
    },"t2").start();
    
    sleep(0.5); 
    log.debug("唤醒 obj 上其它线程");
    synchronized (obj) { 
      obj.notifyAll(); // 唤醒obj上所有等待线程 
    }
  }
}
```














































