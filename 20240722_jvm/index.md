# JVM 原理拙见


<!--more-->

## 概念

`JVM` 是 Java 的核心和基础，处于 Java 编译器和 OS 平台之间的虚拟处理器。它是一种利用软件方法实现的抽象的计算机基于下层的操作系统和硬件平台，可以在上面执行 Java 的字节码程序。Java 编译器只要面向 `JVM`，生成 `JVM` 能够理解的代码或字节码文件。Java 源文件经编译成字节码程序，通过 `JVM` 将每一条指令翻译成不同平台机器码，通过特定的平台运行。

<br>

## JVM的生命周期

1. **JVM 的诞生**：当启动一个 Java 程序时，一个 JVM 实例就产生了，任何一个拥有`public static void main(String[] args)`函数的 class 都可以作为 JVM 实例运行的起点。
2. **JVM 实例的运行**：main() 作为该程序初始线程的起点，任何其他线程均由该线程启动。JVM 内部有两种线程：**守护线程**和**非守护线程**，main() 属于非守护线程，守护线程通常由 JVM 自己使用，Java 程序也可以标明自己创建的线程是守护线程。
3. **JVM 实例的消亡**：当程序中的所有非守护线程都终止时，JVM 才退出；若安全管理器允许，程序也可以使用 java.lang.Runtime 类或者 java.lang.System.exit() 来退出。

<br>

## JVM运行内存模型

概念：JVM 把文件 .class 字节码加载到内存，对数据进行校验，转换和解析，并初始化，最终形成可以被虚拟机直接使用的 Java 类型，这就是虚拟机的类加载机制。

![类加载机制](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6.jpeg "类加载机制")

<br>

### 类装载器

> 类加载器的任务是根据一个类的全限定名来读取此类的二进制字节流到 JVM 中，然后转换为一个与目标对应的 java.lang.Class 对象实例

<br>

{{< admonition tip 虚拟机提供的四类类加载机制>}}

1. **启动类加载器（Bootstrap ClassLoader）** C++ 编写，加载 Java 核心 java.*
    - 这个类加载器负载放在 <JAVA_HOME>/lib 目录中的，或者被 -Xbootclasspath 参数所指定的路径中，并且是虚拟机识别的类库。用户无法直接使用。
    - 由于引导类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的引用，所以不允许直接通过引用进行操作。
2. **扩展类加载器（Extension ClassLoader）** Java 编写，加载扩展库
    - 这个类加载器由 sun.misc.Launcher$AppClassLoader 实现。它负责 <Java_Home>/lib/ext 目录中的jar文件，或者被 java.ext.dirs 系统变量所指定的路径中的所有类库。用户可以直接使用。

3. **应用程序类加载器（Application ClassLoader）** Java 编写，加载程序所在的目录
    - 这个类由 sun.misc.Launcher$AppClassLoader 实现。是 ClassLoader 中 getSystemClassLoader() 方法的返回值。它负责用户路径（ClassPath）所指定的类库。用户可以直接使用。如果用户没有自己定义类加载器，默认使用这个。

4. **自定义类加载器（User ClassLoader）**
    - 用户自己定义的类加载器。


{{< /admonition >}}

<br>

### 原理

类从被加载到虚拟机内存中开始，到卸载出内存为止，它的生命周期包括一下流程：

![类加载原理](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%8E%9F%E7%90%86.jpeg "类加载原理")

#### 加载（重点）

加载阶段是“类加载机制”中的一个阶段，这个阶段通常也被称作“装载”，主要完成：

1. 通过“类全名”来获取定义此类的二进制字节流
2. 将字节流所代表的静态存储结构转换为方法区中的运行时数据结构--模版
3. 在 Java 堆中生成一个代表这个类的 java.lang.Class 对象，作为方法区这些数据的访问入口

{{< admonition quote 解释 >}}

- 相对于类加载过程中的其他阶段，加载阶段（准确来说，是加载阶段中获取类的二进制字节流的动作）是开发期可控性最强的阶段，因为加载阶段可以使用系统提供的类加载器（`ClassLoader`）来完成，也可以由用户自定义的类加载器完成，开发人员可以通过定义自己的类加载器去控制字节流的获取方式。
- 加载阶段完成后，虚拟机外部的二进制字节流就按照虚拟机所需的格式存储在方法区之中，方法区中的数据存储格式由虚拟机实现自行定义，虚拟机并未规定此区域的具体数据结构。然后在 Java 堆中实例化一个 java.lang.Class 类的对象，这个对象作为程序访问方法区中的这些类型数据的外部接口。

{{< /admonition >}}

#### 验证

验证是链接阶段的第一步，这一步主要的目的是确保 class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身安全。

验证阶段主要包括四个检验过程：文件格式验证、元数据验证、字节码验证和符号引用验证。

#### 准备

准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些内存都将在方法区中进行分配。这个阶段中有两个容易混淆的知识点，

首先是这个时候进行内存分配的变量仅包括类变量（`static` 修饰的变量），而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在 Java 堆中。

其次是这里所说的初始值“通常情况”下是数据类型的零值，假设一个类变量定义为：（**public static int value = 12;**）那么变量 value 在准备阶段过后的初始值为0而不是12，因为这个时候尚未开始执行任何 Java 方法，而把 valve 赋值为12的指令是被程序编译后，存放在类构造器()方法之中，所以把 value 赋值为12的动作将在初始化阶段才会被执行。

上面所说的“通常情况”下初始值是零值，那相对一些特殊情况，如果类字段的字段属性表中存在 **ConstantValue** 属性，那在准备阶段变量 value 就会被初始化为 **ConstantValue** 属性所指定的值，建议上面类变量 value 定义为(public static final int value = 12；)编译时 Java 将会为 value 生成 **ConstantValue** 属性，在准备阶段虚拟机就会根据 **ConstantValue** 的设置将 value 设置为12。

#### 解析

解析阶段是虚拟机常量池内的符号引用替换为直接引用的过程

**符号引用：** 符号引用是一组符号来描述所引用的目标对象，符号可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可。符号引用与虚拟机实现的内存布局无关，引用的目标对象并不一定已经加载到内存中。

**直接引用：** 直接引用可以是直接指向目标对象的指针、相对偏移量或是一个能间接定位到目标的句柄。直接引用是与虚拟机内存布局实现相关的，同一个符号引用在不同虚拟机实例上翻译出来的直接引用一般不会相同，如果有了直接引用，那引用的目标必定是已经在内存中存在。

解析的动作主要准对类或接口、字段、类方法、接口方法四类符号引用进行。分别对应编译后常量池内的 CONSTANT_CLASS_INFO、CONSTANT_FIELDREF_INFO、CONSTANT_METHODEF_INFO、CONSTANT_INTERFACEMETHODER_INFO 四种常量类型。

1. 类、接口的解析
2. 字段解析
3. 类方法解析
4. 接口方法解析

#### 初始化

类加载最后阶段，若该类具有超类，则对其进行初始化，执行静态初始化器和静态初始化成员变量（如前面只初始化了默认值的 static 变量将会在这个阶段赋值，成员变量也将被初始化）。

<br>

### 类装载器内部机制

#### 原理

当某个类加载器需要加载某个.class文件时，它首先会把这个任务委托给它的上级类加载器，递归这个操作，如果上级的类加载器没有加载，自己才会去加载这个类。

![类加载流程](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%B5%81%E7%A8%8B.jpeg "类加载流程")

抽象理解：即每个儿子都不愿意干活，每次有活就丢给父亲去干，直到父亲说这件事我干不了时，儿子自己才会想办法去完成，这就是传说中的**双亲委派机制** 。

#### 双亲委派机制作用

1. **保证安全性**

    防止重复加载同一个 class。通过委托去向上面问，加载过了就不用再加载一遍。保证数据安全。

2. **类加载流程**

    保证核心 .class 不能被篡改。通过委托方式，就不会去篡改核心 .class，即使篡改也不会去加载，即使加载也不会是同一个 .class 对象。不同的加载器加载同一个 .class 也不是同一个 Class 对象。

解释：如果没有双亲委派模型，而是由各个类加载器自行加载，如果用户编写了一个 java.lang.Object 的同名类并放在 ClassPath 中，多个类加载器都去加载这个类到内存中，系统中将会出现多个不同的 Object 类，那么类之间的比较结果以及类的唯一性将无法保证，因为 Object 都各自不相同，那么程序运行启动就会出错，所以双亲委派模型也保证了 JVM 能够正常安全运行。

<br>

### 运行时数据区

![JVM运行数据区](https://cdn.jsdelivr.net/gh/Turbo-King/images/JVM%E8%BF%90%E8%A1%8C%E6%95%B0%E6%8D%AE%E5%8C%BA.jpeg "JVM运行时数据区")

{{< admonition quote 介绍>}}

1. JVM 运行时会分配好方法区和堆，而 JVM 每次遇到一个线程，就为其分配一个程序计数器、Java栈、本地方法栈，当线程终止时，三者（`程序计数器、Java栈、本地方法栈`）所占用的内存空间也会释放掉。
2. 程序计数器、Java栈、本地方法栈的生命周期与所属线程相同，而方法区和堆的生命周期与 Java 程序运行生命周期相同，所以 GC 只发生在线程共享的区域（大部分发生在 `HEAP` 上）。

{{< /admonition >}}

<br>

#### 堆 - 线程共享

- **Java 中的堆是用来存储对象实例以及数组**（当然，数组引用是存放在 Java 栈中的）。**堆是被所有线程共享的，因此在其上进行对象内存的分配均需要进行加锁，这也导致了 new 对象的开销是比较大的。** 在 JVM 中只有一个堆。堆是 Java 垃圾收集器管理的主要区域，Java 的垃圾回收机制会自动进行处理。
- **堆空间分为老年代和年轻代**。刚刚创建的对象存放在年轻代，而老年代中存放生命周期长久的实例对象。
- **年轻代中又被分为 `Eden` 区和两个 `Survivor` 区。新的对象分配首先是放在 Eden 区，Survivor 区作为 Eden 区和 `Old` 区的缓冲，在 Survivor 区的对象经历若干次 GC 仍然存活的，就会被转移到老年代。** 当一个对象大于 Eden 区而小于 Old 区（老年代）的时候就会直接扔到 Old 区。而当对象大于 Old 区时。会直接抛出 `OutOfMemoryError（OOM）`

<br>

#### 元空间 = 永久代（方法区） - 线程共享

存储在物理硬盘上（非堆）

- **有时候也称为永久代（Permanent Generation）**，在方法区中，存储了每个类的信息 **（包括：类的名称、修饰符、方法信息、字段信息）、类中静态变量、类中定义为 final 类型的常量、类中的 Field 信息、类中的方法信息以及编译器编译后的代码等**
- **当开发人员在程序中通过 Class 对象中的 `getName`、`isInterface` 等方法来获取信息时，这些数据都来源于方法区域，同时方法区域也是全局共享的，在一定的条件下它也会被 `GC`**，在这里进行的 GC 主要是方法区里的常量池和类型的卸载。当方法区域需要使用的内存超过其允许的大小时，会抛出 **OutOfMemory（OOM）** 的错误信息
- **方法区中有一个非常重要的部分就是运行时常量池，用于存放静态编译产生的字面量和符号引用。运行时生成的常量也会存在这个常量池中**，比如 String 的 intern 方法。它是每一个类或接口的常量池的运行时表现形式，在类和接口被加载到 JVM 后，对应的运行时常量池就被创建出来。

![JVM堆内存结构](https://cdn.jsdelivr.net/gh/Turbo-King/images/JVM%E5%A0%86%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84.png "JVM堆内存结构")

**为什么元空间又被称为非堆：** 因为在内存模型中它存储的数据和堆有些许不同，所以习惯性把它从堆中单独区分出来。方法区属于非堆内存。它存储每个类结构，如运行时常量池、字段和方法数据，以及方法和构造方法的代码。它是在 Java 虚拟机启动时创建的。

<br>

#### Java栈 - 非线程共享

- **Java栈也称为虚拟机栈（Java Vitual Machine Stack），也就是我们常常所说的栈。JVM 栈是线程私有的，每个线程创建的同时都会创建自己的 JVM 栈，互不干扰。** Java栈是 Java 方法执行的内存模型。
- **Java栈中存放的是一个个栈帧，每个栈帧都对应一个被调用的方法**，在栈帧中包括局部变量表（Local Variables）、操作数（Operand Stark）、指向当前方法所属的类的运行时常量池的引用（Reference to runtime constant pool）、方法返回地址（Retuen Address）和一些额外的附加信息。
- **当线程执行一个方法时，就会随之创建一个对应的栈帧，并将建立的栈帧压栈。当方法执行完毕之后，便会将栈帧出栈。因此可知，线程当前执行的方法所对应的栈帧必定位于 Java 栈的顶部**。
- **局部变量表**：用来存储方法中的局部变量，对于引用类型的变量，则存的是指向对象的引用，对于基本数据类型的变量，则直接存储它的值
- **操作数栈**：栈最典型的一个应用就是用来对表达式求值。在一个线程执行方法的过程中，实际上就是不断执行语句的过程，而归根到底就是进行计算的过程。因此可以这么说，程序中的所有计算过程都是在借助于操作栈来完成的。
- **指向运行时常量池的引用**：因为在方法执行的过程中有可能需要用到类中的常量，所以必须要有一个引用指向运行时常量。
- **方法返回地址**：当一个方法执行完毕之后，要返回之前调用它的地方，因此在栈帧中必须保存一个方法返回地址。

<br>

#### 程序计数器 - 非线程共享

- **程序计数器（Program Counter Register）**，也有称作为 PC 寄存器

    ​	由于在JVM中，多线程是通过线程轮流切换来获得 CPU 执行时间的，因此，在任一具体时刻，一个CPU的内核只会执行一条线程中的指令，因此，为了能够使得每个线程都在线程切换后能够恢复在切换之前的程序执行位置，每个线程都需要有自己独立的程序计数器，并且不能互相被干扰，否则就会影响到程序的正常执行次序。因此，可以这么说，程序计数器是每个线程所私有的。

- 在 JVM 规范中规定，如果线程执行的是非 native（本地）方法，则程序计数器中保存的是当前需要执行的指令的地址；如果线程执行的是 native 方法，则程序计数器中的值是 undefined。
- **由于程序计数器中存储的数据所占空间的大小不会随程序的执行而发生改变**，因此，对于程序计数器是不会发生内存溢出现象（OutOfMemory）的。

<br>

#### 本地方法栈 - 非线程共享

JVM 采用本地方法堆栈来支持 **native** 方法的执行，此区域用于存储每个 native 方法调用的状态。

本地方法栈与 Java 栈的作用和原理非常相似。**区别只不过是 Java栈是为执行 Java 方法服务的，而本地方法栈则是为执行本地方法（Native Method）服务的**。

**JVM 规范中，并没有对本地方法栈的具体实现方法以及数据结构作强制规定，虚拟机可以自由实现它。在 `HotSopt` 虚拟机中直接就把本地方法栈和 Java栈合二为一**

<br>

### 创建对象内存分析

创建一个Dog类，实例化两个对象，分别是"AHuang"和"AHui"。

```java
public class Dog{
		String name;
		int age;
		
		public void run(){
			System.out.println("running");
		}
}
```

```java
public class Test{

  public static void main(String[] args){
    Dog Ahuang = new Dog();
    Ahuang.name = "Ahuang";
    Ahuang.age = 1;
    Ahuang.shout;

    Dog AHui = new Dog();
    AHui.name = "AHui";
    AHui.age = 5;
    AHui.shout;
  }
}

```

![创建对象内存分析](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E5%86%85%E5%AD%98%E5%88%86%E6%9E%90.jpeg "创建对象内存分析")

首先会在方法区加载 Class 模版，每个模版都有该类的属性以及方法和常量池，以及静态变量随着类加载而加载到静态方法区，并在 Java 堆中生成一个代表该类的 Class 对象的内存区域，为类变量分配内存并设置类变量初始值，例如 String 类型为 null，而后在栈中存储该对象的引用地址，地址指向堆中类的内存地址，最后初始化，例如构造方法，复制给堆中的对象内存数据中。每一个线程都会给它分配指定的 Java 栈，程序计数器、本地方法栈，所以说这三个是线程独享的，是线程安全的，而堆和方法区大家一起使用，因而线程不安全。

<br>

## 总结

东西扯了这么久，也参考了一些资料，也有自己在学习上的一些总结，这里面也埋了一个彩蛋，关于JVM GC操作的执行流程又是怎样的呢？希望以上对你有所帮助，谢谢。




