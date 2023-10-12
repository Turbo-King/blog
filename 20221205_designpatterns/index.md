# 设计模式


# 浅谈设计模式

<!--more-->

## 前言

> 编写软件过程中，程序员面临着来自**耦合性**，**内聚性**以及**可维护性**，**可扩展性**，**重用性**，**灵活性** 等多方面的挑战，设计模式是为了让程序(软件)，具有更好：
>
> 1. **代码重用性**（即：相同的代码，不用多次编写）
> 2. **可读性**（即：编程规范性，便于其他程序员的阅读与理解）
> 3. **可扩展性**（即：当需要增加新的功能时，非常方便，称为可维护性）
> 4. **可靠性**（即：当我们增加新的功能后，对原来的功能没有影响）
> 5. 使程序呈现**高内聚，低耦合**的特性

设计模式包含了面向对象的精髓：“懂了设计模式，你就懂了面向对象分析和设计（OOA/D）的精要”。

Scott Mayers 在其巨著《Effective C++》就曾经说过：C++老手和 C++新手的区别就是前者手背上有很多伤疤。

<br><br><br>

## 设计模式七大原则

设计模式原则，其实就是程序员在编程时，应当遵守的原则，也是各种设计模式的基础（即：设计模式为什么这样设计的依据）

{{< admonition abstract 设计模式常用的七大原则 >}}

1. **单一职责原则**
2. **接口隔离原则**
3. **依赖倒转（倒置）原则**
4. **里氏替换原则**
5. **开闭原则**
6. **迪米特法则**
7. **合成服用原则**

{{< /admonition >}}



### 单一职责原则

#### 介绍

对类来说，一个类应该只负责一项职责。如类A负责两个不同的职责：职责1，职责2。当职责1需求变更而改变A时，可能造成职责2执行错误，所以需要将类A的粒度分解为A1，A2。类图如下所示

![单一职责原则](https://cdn.jsdelivr.net/gh/Turbo-King/images/单一职责.png.jpeg "单一职责原则")



#### 单一职责原则注意事项和细节

{{< admonition tip 单一职责原则 >}}

1. 降低类的复杂度，一个类只负责一项职责。
2. 提高类的可读性，可维护性。
3. 降低变更引起的风险。
4. 通常情况下，我们应当遵守单一职责原则，只有逻辑足够简单，才可以在代码级违反单一职责原则；只有类中的方法数量足够少，可以在方法级别保持单一职责原则。

{{< /admonition >}}



### 接口隔离原则

#### 介绍

客户端不应该依赖他不需要的接口，即对类对另一个类的依赖应该建立在最小的接口上。

![接口隔离原则-未拆分](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2022-12-05%2014.40.01.png "接口隔离原则-未拆分")

类A通过接口Interface1依赖类B，类C通过Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须去实现他们不需要的方法。

**隔离原则应当如下处理：**

将接口Interface1拆分成为几个独立的接口（这里我们拆分成为3个接口），类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则。



#### 使用接口隔离原则改进

{{< admonition example  步骤>}}

1. 类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须去实现他们不需要的方法
2. 将接口Interface1拆分为独立的几个接口，类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则
3. 接口Interface1中出现的方法，根据实际情况拆分为三个接口

{{< /admonition >}}

拆分实现如下：

![接口隔离原则-拆分](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2022-12-05%2014.56.50.png "接口隔离原则-拆分")

```java
public class Segregation {
    public static void main(String[] args) {
        A a = new A();
        a.depend1(new B()); // A类通过接口去依赖B类
        a.depend2(new B());
        a.depend3(new B());

        C c = new C();
        c.depend1(new D()); // C类通过接口去依赖D类
        c.depend4(new D());
        c.depend5(new D());

    }
}

interface Interface1 {
    void operation1();
}

interface Interface2 {
    void operation2();

    void operation3();
}

interface Interface3 {
    void operation4();

    void operation5();
}

class D implements Interface1, Interface3 {

    @Override
    public void operation1() {
        System.out.println("D 实现了 operation1");
    }

    @Override
    public void operation4() {
        System.out.println("D 实现了 operation4");

    }

    @Override
    public void operation5() {
        System.out.println("D 实现了 operation5");

    }
}

class B implements Interface1, Interface2 {

    @Override
    public void operation1() {
        System.out.println("B 实现了 operation1");
    }

    @Override
    public void operation2() {
        System.out.println("B 实现了 operation2");

    }

    @Override
    public void operation3() {
        System.out.println("B 实现了 operation3");

    }
}

// A类通过接口Interface依赖使用B类，用到了1，2，3方法
class A {
    public void depend1(Interface1 interface1) {
        interface1.operation1();
    }

    public void depend2(Interface2 interface2) {
        interface2.operation2();
    }

    public void depend3(Interface2 interface2) {
        interface2.operation3();
    }
}

// C类通过接口Interface依赖D类，用到了1，4，5方法
class C {
    public void depend1(Interface1 interface1) {
        interface1.operation1();
    }

    public void depend4(Interface3 interface3) {
        interface3.operation4();
    }

    public void depend5(Interface3 interface3) {
        interface3.operation5();
    }
}
```



### 依赖倒转原则

#### 介绍

依赖倒转原则(Dependence Inversion Principle)是指：

1. 高层模块不应该依赖底层模块，二者都应该依赖其抽象

2. 抽象不应该依赖细节，细节应该依赖抽象

3. 依赖倒转(倒置)的中心思想是面向接口编程

4. 依赖倒转原则是基于这样的设计原理：相较于细节的多变性，抽象的东西要稳定的多。以抽象为基础搭建的框架比以细节为基础的框架要稳定的多。在Java中，抽象指的是借口或抽象类，细节就是具体的实现类

5. 使用接口或抽象类的目的是制定好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成

    ![依赖倒转](https://cdn.jsdelivr.net/gh/Turbo-King/images/24a4bef88c92f6af9dbf584f9c382456_r-20230227093317943.jpg "依赖倒转")

    

{{< admonition abstract 依赖关系传递的三种方式 >}}

1. 接口传递
2. 构造方法传递
3. setter方式传递

{{< /admonition >}}

```java
public class DependencyPass {

    public static void main(String[] args) {
        // TODO Auto-generated method stub

    }

}

// 方式1： 通过接口传递实现依赖
// 开关的接口
interface IOpenAndClose {
    public void open(ITV tv); //抽象方法,接收接口
}

interface ITV { //ITV接口
    public void play();
}

// 实现接口
class OpenAndClose implements IOpenAndClose {
    public void open(ITV tv) {
        tv.play();
    }
}

// 方式2: 通过构造方法依赖传递
interface IOpenAndClose {
    public void open(); //抽象方法
}

interface ITV { //ITV接口
    public void play();
}

class OpenAndClose implements IOpenAndClose {
    public ITV tv;

    public OpenAndClose(ITV tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}

// 方式3 , 通过setter方法传递
interface IOpenAndClose {
    public void open(); // 抽象方法

    public void setTv(ITV tv);
}

interface ITV { // ITV接口
    public void play();
}

class OpenAndClose implements IOpenAndClose {
    private ITV tv;

    public void setTv(ITV tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}
```

#### 依赖倒转原则的注意事项和细节

{{< admonition tip 依赖倒转原则 >}}

1. 底层模块尽量都要有抽象类或接口，或者两者都有，程序稳定性更好
2. 变量的声明类型尽量是抽象类或接口，这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化
3. 继承时遵循里氏替换原则

{{< /admonition >}}

### 里氏替换原则

{{< admonition question OO中的继承性的思考和说明 >}}

1. 继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是在设定规范和契约，虽然他不强制要求所有的子类必须遵循这些契约，但是如果自类对这些已经实现的方法随意修改，就会对整个继承体系造成破坏
2. 继承在给程序设计带来便利的同时，也带来了弊端。比如使用继承会给程序带来侵入性，程序的可移植性降低，增加对象的耦合性，如果一个类被其他的类所继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障

{{< /admonition >}}

#### 介绍

1. 里氏替换原则(Liskov Substitution Principle)在1988年，由麻省理工学院里一位姓里的女士提出的

2. 如果对每个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有的对象o1都替换成o2时，程序P的行为没有发生变化，那么类型T2是类型T1的子类型。换句话说，所有的引用基类的地方必须能透明地使用其子类的对象
3. 在使用继承，遵循里氏替换原则，在**子类中尽量不要重写父类的方法**
4. 里氏替换原则告诉我们，继承实际上让两个类耦合性增强了，在适当的情况下，可以通过**耦合、组合、依赖**来解决问题

![里式替换原则](https://cdn.jsdelivr.net/gh/Turbo-King/images/111901060-b86c2f00-8a70-11eb-99fb-f28d28c62cb4.png "里氏替换")

**里氏替换原则通用做法：原来的父类和子类都继承一个更通俗的基类，原有的继承关系去掉，采用依赖、聚合、组合等关系替代。**



### 开闭原则

#### 介绍

1. 开闭原则(Open Closed Principle)是编程中最基础、最重要的设计原则
2. 一个软件实体如类，模块和函数应该对扩展开放(对提供方)，对修改关闭(对使用方)。用抽象构建框架，用实现扩展细节
3. 当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化
4. 编程中遵循其他原则，以及使用设计模式的目的就是遵循开闭原则

![代码演示](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-02%2021.09.58.png "类图演示")



```Java
public class Ocp {

	public static void main(String[] args) {
		//使用看看存在的问题
		GraphicEditor graphicEditor = new GraphicEditor();
		graphicEditor.drawShape(new Rectangle());
		graphicEditor.drawShape(new Circle());
		graphicEditor.drawShape(new Triangle());
	}

}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
	//接收Shape对象，然后根据type，来绘制不同的图形
	public void drawShape(Shape s) {
		if (s.m_type == 1)
			drawRectangle(s);
		else if (s.m_type == 2)
			drawCircle(s);
		else if (s.m_type == 3)
			drawTriangle(s);
	}

	//绘制矩形
	public void drawRectangle(Shape r) {
		System.out.println(" 绘制矩形 ");
	}

	//绘制圆形
	public void drawCircle(Shape r) {
		System.out.println(" 绘制圆形 ");
	}
	
	//绘制三角形
	public void drawTriangle(Shape r) {
		System.out.println(" 绘制三角形 ");
	}
}

//Shape类，基类
class Shape {
	int m_type;
}

class Rectangle extends Shape {
	Rectangle() {
		super.m_type = 1;
	}
}

class Circle extends Shape {
	Circle() {
		super.m_type = 2;
	}
}

//新增画三角形
class Triangle extends Shape {
	Triangle() {
		super.m_type = 3;
	}
}
```



#### 演示方式优缺点

1. 代码易于理解，操作简单
2. 缺点是违反了设计模式的OCP原则，即**对扩展开放（提供方），对修改关闭（适用方）**。即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码
3. 比如我们这时要新增加一个图形种类三角形，我们需要修改较多地方



{{< admonition example 优化分析 >}}

把创建Shape类做成抽象类，并提供一个抽象的draw方法，让子类去实现即可，这样我们有新的图形种类时，只需要让新的图形类继承Shape，并实现draw方法即可。（使用方的代码就不需要修改，即可满足开闭原则）

{{< / admonition >}}

```Java
public class Ocp {

	public static void main(String[] args) {
		//使用看看存在的问题
		GraphicEditor graphicEditor = new GraphicEditor();
		graphicEditor.drawShape(new Rectangle());
		graphicEditor.drawShape(new Circle());
		graphicEditor.drawShape(new Triangle());
		graphicEditor.drawShape(new OtherGraphic());
	}

}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
	//接收Shape对象，调用draw方法
	public void drawShape(Shape s) {
		s.draw();
	}

	
}

//Shape类，基类
abstract class Shape {
	int m_type;
	
	public abstract void draw();//抽象方法
}

class Rectangle extends Shape {
	Rectangle() {
		super.m_type = 1;
	}

	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制矩形 ");
	}
}

class Circle extends Shape {
	Circle() {
		super.m_type = 2;
	}
	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制圆形 ");
	}
}

//新增画三角形
class Triangle extends Shape {
	Triangle() {
		super.m_type = 3;
	}
	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制三角形 ");
	}
}

//新增一个图形
class OtherGraphic extends Shape {
	OtherGraphic() {
		super.m_type = 4;
	}

	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制其它图形 ");
	}
}
```



### 迪米特法则

#### 介绍

1. 一个对象应该对其他对象保持最少的了解
2. 类和类关于越密切，耦合度就越大
3. 迪米特法则(Demeter Principle)又叫最少知道原则，即一个类对自己依赖的类知道的越少越好。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的public方法，不对外泄露任何信息
4. 迪米特法则还有个更简单的定义：只与直接的朋友通信
5. **直接的朋友**：每个对象都会与其他对象有耦合关系，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式有很多：依赖、关联、组合、聚合等。其中，我们成出现的成员变量，方法参数，方法返回值中的类为直接的朋友，而出现在局部变量中的类不是直接的朋友。也就是说，陌生的类最好不要以局部变量的形式出现在类的内部

#### 实例应用

**描述**：有一个学校，下属有各个学院和总部，现要求打印出学校总部员工ID和学院员工的ID

```Java
import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter {

	public static void main(String[] args) {
		//创建了一个 SchoolManager 对象
		SchoolManager schoolManager = new SchoolManager();
		//输出学院的员工id 和  学校总部的员工信息
		schoolManager.printAllEmployee(new CollegeManager());

	}

}


//学校总部员工类
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//学院的员工类
class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//管理学院员工的管理类
class CollegeManager {
	//返回学院的所有员工
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) { //这里我们增加了10个员工到 list
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
	//返回学校总部的员工
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) { //这里我们增加了5个员工到 list
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	//该方法完成输出学校总部和学院员工信息(id)
	void printAllEmployee(CollegeManager sub) {
		
		//分析问题
		//1. 这里的 CollegeEmployee 不是  SchoolManager的直接朋友
		//2. CollegeEmployee 是以局部变量方式出现在 SchoolManager
		//3. 违反了 迪米特法则 
		
		//获取到学院员工
		List<CollegeEmployee> list1 = sub.getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
		//获取到学校总部员工
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}
```

{{< admonition example 应用实例改进 >}}

1. 前面设计的问题在于SchoolManager中，CollegeEmployee类并不是SchoolManager类的直接朋友
2. 按照迪米特法则，应该避免类中出现这样非直接朋友关系的耦合

{{< / admonition >}}

```Java
import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter {

	public static void main(String[] args) {
		System.out.println("~~~使用迪米特法则的改进~~~");
		//创建了一个 SchoolManager 对象
		SchoolManager schoolManager = new SchoolManager();
		//输出学院的员工id 和  学校总部的员工信息
		schoolManager.printAllEmployee(new CollegeManager());

	}

}


//学校总部员工类
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//学院的员工类
class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//管理学院员工的管理类
class CollegeManager {
	//返回学院的所有员工
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) { //这里我们增加了10个员工到 list
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
	
	//输出学院员工的信息
	public void printEmployee() {
		//获取到学院员工
		List<CollegeEmployee> list1 = getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
	}
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
	//返回学校总部的员工
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) { //这里我们增加了5个员工到 list
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	//该方法完成输出学校总部和学院员工信息(id)
	void printAllEmployee(CollegeManager sub) {
		
		//分析问题
		//1. 将输出学院的员工方法，封装到CollegeManager
		sub.printEmployee();
	
		//获取到学校总部员工
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}
```

{{< admonition tip 迪米特法则注意事项和细节 >}}

1. 迪米特法则的核心是降低类之间的耦合
2. 但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低类间（对象间）耦合关系，并不是要求完全没有依赖关系

{{< /admonition >}}



### 合成复用原则

#### 介绍

原则是尽量使用合成/聚合的方式，而不是使用继承

![合成复用原则](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-05%2021.05.08.png "合成复用原则")

{{< admonition abstract 设计原则核心思想 >}}

1. 找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起
2. 针对接口编程，而不是针对实现编程
3. 为了交互对象之间的松耦合设计而努力

{{< /admonition >}}

<br><br><br>

## UML类图

### 介绍

1. UML--Unified modeling language UML（统一建模语言），是一种用于软件系统分析和设计的语言工具，它用于帮助软件开发人员进行思考和记录思路的结果
2. UML本身是一套符号的规定，就像数学符号和化学符号一样，这些符号用于描述软件模型中的各个元素和他们之间的关系，比如类、接口、实现、泛化、依赖、组合、聚合等

![类关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-05%2023.01.54.png "类关系")

<br>

{{< admonition tip UML图例分类 >}}

1. 用例图（use case）
2. 静态结构图：**类图**、对象图、包图、组件图、部署图
3. 动态行为图：交互图（时序图与协作图）、状态图、活动图

**说明**：

1. **类图用于描述系统中类（对象）本身的组成和类（对象）之间的各种静态关系，是UML图中最核心的图例**
2. **设计模式中使用类图描述模式设计过程**
3. **类之间关系：依赖、泛化（继承）、实现、关联、聚合与组合**

{{< /admonition >}}

### 依赖关系（Dependence）

只要在类中使用到了对方，那么他们之间就存在依赖关系。如果没有对方，连编译都通过不了。

![依赖关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2010.59.17.png "依赖关系")

### 泛化关系（Generalization）

如果一个模型元素（子代）基于另一个模型元素（父代），那么这两个元素之间就存在泛化关系（**继承关系**）

![泛化关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2011.04.28.png "泛化关系")

### 实现关系（Implementation）

实现关系实际上是A类实现B接口，它是**依赖关系的特例**

![实现关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2011.10.37.png "实现关系")

### 关联关系（Association）

关联关系实际上是类与类之间的联系，它是**依赖关系的特例**

关联具有**导航性**：即双向关系或单向关系

关系具有**多重性**：如“1”（表示有且仅有一个），“0...”（表示0个或者多个），“0，1”（表示0个或者一个），“n...m”（表示n到m个都可以），“m...*”（表示至少m个）

![关联关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2022.59.32.png "关联关系")

<br>

### 聚合关系（Aggregation）

是整体与部分的关系, 且部分可以离开整体而单独存在。

聚合关系是关联关系的一种，是强的关联关系

关联和聚合在语法上无法区分，必须考察具体的逻辑关系

**如下所示**：车和轮胎是整体和部分的关系，轮胎离开车仍然可以存在

![聚合关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2022.52.13.png "聚合关系")

<br>

### 组合关系（Composition）

组合关系：也是整体与部分的关系，但是**整体与部分不可以分开**

组合关系是关联关系中的一种，也是比聚合关系还要强的关系，它要求普通的聚合关系中代表整体的对象负责代表部分对象的生命周期

**如下所示**：公司和部分是整体和部门的关系，没有公司就不存在部门

![组合关系](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2023.01.18.png "组合关系")

<br>

<br>

<br>

## 设计模式

### 介绍

1. 设计模式是程序员在面对同类软件工程设计问题所总结出来的有用的经验，模式不是代码，而是某类问题的通用解决方案，设计模式（Design Pattern）代表了**最佳的实践**。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。
2. 设计模式的本质是提高软件的维护性、通用型和扩展性，并降低软件的复杂度。
3. 《设计模式》是经典的书，作者是Erich Gamma、Richard Helm、Ralph Johnson和John Vlissides Design（俗称“四人组GOF”）
4. 设计模式并不局限于某种语言，Java、PHP、C++都有设计模式。

### 设计模式分类

{{< admonition abstract 模式分为三种类型 共23种 >}}

1. **创建型模式：** **单例模式**、抽象工厂模式、原型模式、建造者模式、**工厂模式**。
2. **结构型模式：** 适配器模式、桥接模式、**装饰模式**、组合模式、外观模式、享元模式、**代理模式**。
3. **行为型模式：** 模版方法模式、命令模式、访问者模式、迭代器模式、**观察者模式**、中介者模式、备忘录模式、解释器模式（Interpreter模式）、状态模式、策略模式、职责模式（责任链模式）。

{{< /admonition >}}

### 单例模式

#### 介绍

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类中**只能存在一个对象实例**，并且该类只提供一个取得其对象实例的方法（静态方法）。

<br>

**单例模式举例：** 比如Hibernate的SessionFactory，它充当数据存储源的代理，并负责创建Session对象。SessionFactory并不是一个轻量级的，一般情况下，一个项目通常只需要一个SessionFactory就够，这时就会使用到单例模式。

{{< admonition example 单例模式的八种方式 >}}

1. **饿汉式（静态常量）**
2. **饿汉式（静态代码块）**
3. 懒汉式（线程不安全）
4. 懒汉式（线程安全，同步方法）
5. 懒汉式（线程安全，同步代码块）
6. **双重检查**
7. **静态内部类**
8. **枚举**

{{< / admonition >}}

#### 饿汉式（静态常量）

##### 实现步骤

1. 构造器私有化（防止new）
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法
4. 代码实现

```java
public class SingletonTest01 {

	public static void main(String[] args) {
		//测试
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

//饿汉式(静态变量)

class Singleton {
	
	//1. 构造器私有化, 外部能new
	private Singleton() {
		
	}
	//2.本类内部创建对象实例
	private final static Singleton instance = new Singleton();
	
	//3. 提供一个公有的静态方法，返回实例对象
	public static Singleton getInstance() {
		return instance;
	}
	
}
```

##### 模式优缺点说明：

1. **优点：** 这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题。
2. **缺点：** 在类装载的时候就完成实例化，没有达到Lazy Loading的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费。
3. 这种方式基于classoder机制避免了多线程的同步问题，不过，instabce在类装载时就实例化，在单例模式中大多数都是调用getInstance方法，但是导致类装载的原因有很多种，因此不能确定有其他的方式（或者其他的静态方法）导致类装载，这个时候初始化instance就没有达到lazy loading的效果。
4. 结论：这种单例模式可用，可能造成内存浪费。

<br>

#### 饿汉式（静态代码块）

##### 实例应用

```java
package com.atguigu.singleton.type2;

public class SingletonTest02 {

	public static void main(String[] args) {
		//测试
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

//饿汉式(静态变量)

class Singleton {
	
	//1. 构造器私有化, 外部能new
	private Singleton() {
		
	}
	
	//2.本类内部创建对象实例
	private  static Singleton instance;
	
	static { // 在静态代码块中，创建单例对象
		instance = new Singleton();
	}
	
	//3. 提供一个公有的静态方法，返回实例对象
	public static Singleton getInstance() {
		return instance;
	}
	
}
```

##### 模式优缺点说明

1. 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。（优缺点与上面一致）
2. 结论：这种单例模式可用，但是可能造成内存浪费。

<br>

#### 懒汉式（线程不安全）

##### 实例应用

```java
public class SingletonTest03 {

	public static void main(String[] args) {
		System.out.println("懒汉式1 ， 线程不安全~");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

class Singleton {
	private static Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，当使用到该方法时，才去创建 instance
	//即懒汉式
	public static Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

##### 模式优缺点说明

1. 起到了Lazy Loading的效果，但是只能在单线程下使用。
2. 如果在多线程下，一个线程进入了if(singleton == null)判断语句块，还未来得及往下执行，另一个线程也通过了这个判断语句，这时便会产生多个实例。所以在多线程环境下不可以使用这种方式。
3. 结论：在实际开发中，不要使用这种方式。

<br>

#### 懒汉式（线程安全，同步方法）

##### 应用实例

```java
public class SingletonTest04 {

	public static void main(String[] args) {
		System.out.println("懒汉式2 ， 线程安全~");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

// 懒汉式(线程安全，同步方法)
class Singleton {
	private static Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，加入同步处理的代码，解决线程安全问题
	//即懒汉式
	public static synchronized Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

##### 模式优缺点说明

1. 解决了线程不安全问题
2. 效率太低了，每个线程再想获得类的实例时候，执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例化代码就够了，后面的想获得该类实例，直接return就行了。方法进行同步效率太低。
3. 结论：在实际开发中，不推荐使用这种方式

<br>

#### 懒汉式（线程安全，同步代码块）

##### 应用实例

```java
class Singleton {
	private static Singleton singleton;
	
	private Singleton() {}

	public Singleton getInstance() {
		if(singleton == null) {
			synchronized (Singleton.class) {
				singleton = new Singleton();
			}
		}
		return singleton;
	}
}
// 不推荐使用
```

##### 双重检查

```java
public class SingletonTest06 {

	public static void main(String[] args) {
		System.out.println("双重检查");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
		
	}

}

// 懒汉式(线程安全，同步方法)
class Singleton {
	private static volatile Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，加入双重检查代码，解决线程安全问题, 同时解决懒加载问题
	//同时保证了效率, 推荐使用
	
	public static synchronized Singleton getInstance() {
		if(instance == null) {
			synchronized (Singleton.class) {
				if(instance == null) {
					instance = new Singleton();
				}
			}
			
		}
		return instance;
	}
}
```

##### 模式优缺点说明

1. Double-Check概念是多线程开发中常使用到的，如代码中所示，我们进行了两次if（singleton==null），这样就可以保证线程安全了。
2. 这样，实例化代码只用执行一次，后面再次访问时，判断if（singleton==null），直接return实例化对象，也避免了反复进行方法同步。
3. 线程安全；延迟加载；效率较高；
4. 结论：在实际开发中，推荐使用这种单例设计模式。

<br>

#### 静态内部类

##### 应用实例

```java
public class SingletonTest07 {

	public static void main(String[] args) {
		System.out.println("使用静态内部类完成单例模式");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
		
	}

}

// 静态内部类完成， 推荐使用
class Singleton {
	private static volatile Singleton instance;
	
	//构造器私有化
	private Singleton() {}
	
	//写一个静态内部类,该类中有一个静态属性 Singleton
	private static class SingletonInstance {
		private static final Singleton INSTANCE = new Singleton(); 
	}
	
	//提供一个静态的公有方法，直接返回SingletonInstance.INSTANCE
	
	public static synchronized Singleton getInstance() {
		
		return SingletonInstance.INSTANCE;
	}
}
```

##### 模式优缺点说明

1. 这种方式采用了类加载的机制来保证初始化实例时只有一个线程。
2. 静态内部类方式在Singleton类被装载时并不会立即实例化，而是在需要实例化时，调用getInstance方法，才会装载SingletonInstance类，从而完成Singleton的实例化。
3. 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。
4. 优点：避免了线程不安全，利用静态内部类特点实现延迟加载，效率高。
5. 结论：推荐使用。

<br>

#### 枚举

##### 应用实例

```java
public class SingletonTest08 {
	public static void main(String[] args) {
		Singleton instance = Singleton.INSTANCE;
		Singleton instance2 = Singleton.INSTANCE;
		System.out.println(instance == instance2);
		
		System.out.println(instance.hashCode());
		System.out.println(instance2.hashCode());
		
		instance.sayOK();
	}
}

//使用枚举，可以实现单例, 推荐
enum Singleton {
	INSTANCE; //属性
	public void sayOK() {
		System.out.println("ok~");
	}
}
```

##### 模式优缺点说明

1. 这借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
2. 这种方式是Effective Java作者Jsoh Bloch提倡的方式。
3. 结论：推荐使用

<br>

#### 单例模式在JDK应用的源码分析

##### 源码分析

**Java.lang.Runtime就是经典的单例模式（饿汉式）**

```java
public class Runtime {
    private static Runtime currentRuntime = new Runtime();

    /**
     * Returns the runtime object associated with the current Java application.
     * Most of the methods of class <code>Runtime</code> are instance
     * methods and must be invoked with respect to the current runtime object.
     *
     * @return  the <code>Runtime</code> object associated with the current
     *          Java application.
     */
    public static Runtime getRuntime() {
        return currentRuntime;
    }

    /** Don't let anyone else instantiate this class */
    private Runtime() {}
}
// 部分展示
```

{{< admonition tip 单例模式注意事项和细节说明 >}}

1. 单例模式保证了系统内存中该类只存在一个对象，节省了系统资源，对于一些需要频繁创建和销毁的对象，使用单例模式可以提高系统性能。
2. 当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用new。
3. 单例模式使用的场景：需要频繁地进行创建和销毁的对象、创建对象时耗时过多或消耗资源过多（即：**重量级对象**），但又经常用到的对象、**工具类对象**、频繁访问数据库或文件的对象（比如**数据源、session工厂**等）。

{{< /admonition >}}

<br>

### 工厂模式

#### 简单工厂模式

##### 应用举例

需求：有一个披萨的项目，要便于披萨种类的扩展，要便于维护

1. 披萨的种类很多（比如 GreekPizz、CheesePizz等）
2. 披萨的制作有 prepare、back、cut、box
3. 完成披萨店的订购功能

###### 传统方法分析

1. 思路分析（类图）

![披萨项目类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-04-07%2010.26.25.png "披萨项目类图")

简单工厂模式的设计方案：定义一个可以实例化的Pizaa对象的类，封装创建对象的代码。

```java
//将Pizza 类做成抽象
public abstract class Pizza {
	protected String name; //名字

	//准备原材料, 不同的披萨不一样，因此，我们做成抽象方法
	public abstract void prepare();

	
	public void bake() {
		System.out.println(name + " baking;");
	}

	public void cut() {
		System.out.println(name + " cutting;");
	}

	//打包
	public void box() {
		System.out.println(name + " boxing;");
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

```java
public class PepperPizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println(" 给胡椒披萨准备原材料 ");
	}

}
```

```java
public class GreekPizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println(" 给希腊披萨 准备原材料 ");
	}

}
```

```java
public class CheesePizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println(" 给制作奶酪披萨 准备原材料 ");
	}

}
```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


public class OrderPizza {

	Pizza pizza = null;
	String orderType = "";
	// 构造器
	public OrderPizza2() {
		
		do {
			orderType = getType();
			pizza = SimpleFactory.createPizza2(orderType);

			// 输出pizza
			if (pizza != null) { // 订购成功
				pizza.prepare();
				pizza.bake();
				pizza.cut();
				pizza.box();
			} else {
				System.out.println(" 订购披萨失败 ");
				break;
			}
		} while (true);
	}

	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}
}
```

###### 传统方法的优缺点

1. 优点是比较好理解，简单易操作
2. 缺点是违反了设计模式的OCP原则，即**对扩展开放，对修改关闭**。即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码
3. 比如我们这时要新增加一个Pizza的种类（Pepper披萨），我们需要另外增加新种类进行修改

{{< admonition example 改进思路与分析 >}}

**分析：** 修改代码可以接受，但是如果我们在其他的地方有创建Pizza的代码，就意味着，也需要修改，而创建Pizza的代码，**往往有多处**

思路：把创建Pizza对象封装到一个类中，这样我们有新的Pizza种类时，只需要修改该类即可，**其它有创建到Pizza对象的代码就不需要修改了** -> 简单工厂模式

{{< / admonition >}}

##### 简单工厂模式介绍

1. 简单工厂模式是属于创建型模式，是工厂模式的一种。**简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例**。简单工厂模式是工厂模式家族中最简单实用的模式
2. 简单工厂模式：定义了一个创建对象的类，由这个类来封装实例化对象的行为（代码）
3. 在软件开发中，当我们会用到大量的创建某种、某类或者某批对象时，就会使用到工厂模式

###### 简单工厂模式改造

简单工厂模式的设计方案：定义一个可以实例化Pizza对象的类，封装创建对象的代码。

```java
package com.atguigu.factory.simplefactory.pizzastore.order;

//简单工厂类
public class SimpleFactory {

	//更加orderType 返回对应的Pizza 对象
	public Pizza createPizza(String orderType) {

		Pizza pizza = null;

		System.out.println("使用简单工厂模式");
		if (orderType.equals("greek")) {
			pizza = new GreekPizza();
			pizza.setName(" 希腊披萨 ");
		} else if (orderType.equals("cheese")) {
			pizza = new CheesePizza();
			pizza.setName(" 奶酪披萨 ");
		} else if (orderType.equals("pepper")) {
			pizza = new PepperPizza();
			pizza.setName("胡椒披萨");
		}
		
		return pizza;
	}
	
	//简单工厂模式 也叫 静态工厂模式 
	
	public static Pizza createPizza2(String orderType) {

		Pizza pizza = null;

		System.out.println("使用简单工厂模式2");
		if (orderType.equals("greek")) {
			pizza = new GreekPizza();
			pizza.setName(" 希腊披萨 ");
		} else if (orderType.equals("cheese")) {
			pizza = new CheesePizza();
			pizza.setName(" 奶酪披萨 ");
		} else if (orderType.equals("pepper")) {
			pizza = new PepperPizza();
			pizza.setName("胡椒披萨");
		}
		
		return pizza;
	}

}

```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class OrderPizza {

	// 构造器
//	public OrderPizza() {
//		Pizza pizza = null;
//		String orderType; // 订购披萨的类型
//		do {
//			orderType = getType();
//			if (orderType.equals("greek")) {
//				pizza = new GreekPizza();
//				pizza.setName(" 希腊披萨 ");
//			} else if (orderType.equals("cheese")) {
//				pizza = new CheesePizza();
//				pizza.setName(" 奶酪披萨 ");
//			} else if (orderType.equals("pepper")) {
//				pizza = new PepperPizza();
//				pizza.setName("胡椒披萨");
//			} else {
//				break;
//			}
//			//输出pizza 制作过程
//			pizza.prepare();
//			pizza.bake();
//			pizza.cut();
//			pizza.box();
//			
//		} while (true);
//	}

	//定义一个简单工厂对象
	SimpleFactory simpleFactory;
	Pizza pizza = null;
	
	//构造器
	public OrderPizza(SimpleFactory simpleFactory) {
		setFactory(simpleFactory);
	}
	
	public void setFactory(SimpleFactory simpleFactory) {
		String orderType = ""; //用户输入的
		
		this.simpleFactory = simpleFactory; //设置简单工厂对象
		
		do {
			orderType = getType(); 
			pizza = this.simpleFactory.createPizza(orderType);
			
			//输出pizza
			if(pizza != null) { //订购成功
				pizza.prepare();
				pizza.bake();
				pizza.cut();
				pizza.box();
			} else {
				System.out.println(" 订购披萨失败 ");
				break;
			}
		}while(true);
	}
	
	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}

}

```

#### 工厂方法模式

##### 介绍

**工厂方法模式设计方案：** 将披萨项目的实例化功能抽象成抽象方法，在不同的口味点餐子类中具体实现

**工厂方法模式：** 定义了一个创建对象的抽象方法，由子类决定要实例化的类。工厂方法模式将**对象的实例化推迟到子类**

#### 抽象工厂模式

##### 介绍

1. 抽象工厂模式：定义了一个interface用于创建相关或者有依赖关系的对象簇，而无需指明具体的类
2. 抽象工厂模式可以将简单工厂模式和工厂方法模式进行整合
3. 从设计层面看，抽象工厂模式就是对简单工厂模式的改进（或者称为进一步的抽象）
4. 将工厂抽象成两层，AbsFactory（抽象工厂）和具体实现的工厂子类。程序员可以根据创建对象类型实用对应的工厂子类。这样将单个的简单工厂类变成**工厂簇**，更利于代码的维护和扩展

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import com.atguigu.factory.absfactory.pizzastore.pizza.Pizza;

public class OrderPizza {

	AbsFactory factory;

	// 构造器
	public OrderPizza(AbsFactory factory) {
		setFactory(factory);
	}

	private void setFactory(AbsFactory factory) {
		Pizza pizza = null;
		String orderType = ""; // 用户输入
		this.factory = factory;
		do {
			orderType = getType();
			// factory 可能是北京的工厂子类，也可能是伦敦的工厂子类
			pizza = factory.createPizza(orderType);
			if (pizza != null) { // 订购ok
				pizza.prepare();
				pizza.bake();
				pizza.cut();
				pizza.box();
			} else {
				System.out.println("订购失败");
				break;
			}
		} while (true);
	}

	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}
}

```

#### 工厂模式在JDK—Calendar应用的源码分析

JDK中的Calendar类中，就使用了简单工厂模式

```java
package com.atguigu.jdk;

import java.util.Calendar;

public class Factory {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// getInstance 是 Calendar 静态方法
		Calendar cal = Calendar.getInstance();
	    // 注意月份下标从0开始，所以取月份要+1
	    System.out.println("年:" + cal.get(Calendar.YEAR));
	    System.out.println("月:" + (cal.get(Calendar.MONTH) + 1));       
	    System.out.println("日:" + cal.get(Calendar.DAY_OF_MONTH));
	    System.out.println("时:" + cal.get(Calendar.HOUR_OF_DAY));
	    System.out.println("分:" + cal.get(Calendar.MINUTE));
	    System.out.println("秒:" + cal.get(Calendar.SECOND));
	    
	}

}



/**
     * Gets a calendar with the specified time zone and locale.
     * The <code>Calendar</code> returned is based on the current time
     * in the given time zone with the given locale.
     *
     * @param zone the time zone to use
     * @param aLocale the locale for the week data
     * @return a Calendar.
     */
public static Calendar getInstance(TimeZone zone,Locale aLocale)
    {
        return createCalendar(zone, aLocale);
    }

    private static Calendar createCalendar(TimeZone zone,Locale aLocale)
    {
        CalendarProvider provider =
            LocaleProviderAdapter.getAdapter(CalendarProvider.class, aLocale)
                                 .getCalendarProvider();
        if (provider != null) {
            try {
                return provider.getInstance(zone, aLocale);
            } catch (IllegalArgumentException iae) {
                // fall back to the default instantiation
            }
        }

        Calendar cal = null;

        if (aLocale.hasExtensions()) {
            String caltype = aLocale.getUnicodeLocaleType("ca");
            if (caltype != null) {
                switch (caltype) {
                case "buddhist":
                cal = new BuddhistCalendar(zone, aLocale);
                    break;
                case "japanese":
                    cal = new JapaneseImperialCalendar(zone, aLocale);
                    break;
                case "gregory":
                    cal = new GregorianCalendar(zone, aLocale);
                    break;
                }
            }
        }
        if (cal == null) {
            // If no known calendar type is explicitly specified,
            // perform the traditional way to create a Calendar:
            // create a BuddhistCalendar for th_TH locale,
            // a JapaneseImperialCalendar for ja_JP_JP locale, or
            // a GregorianCalendar for any other locales.
            // NOTE: The language, country and variant strings are interned.
            if (aLocale.getLanguage() == "th" && aLocale.getCountry() == "TH") {
                cal = new BuddhistCalendar(zone, aLocale);
            } else if (aLocale.getVariant() == "JP" && aLocale.getLanguage() == "ja"
                       && aLocale.getCountry() == "JP") {
                cal = new JapaneseImperialCalendar(zone, aLocale);
            } else {
                cal = new GregorianCalendar(zone, aLocale);
            }
        }
        return cal;
    }

```

#### 工厂模式小结

1. 工厂模式的意义：将实例化对象的的代码提取出来，放到一个类中统一管理和维护，达到和主项目的依赖关系的解耦。从而提高项目的扩展和维护性。
2. 三种工厂模式（简单工厂模式、工厂方法模式、抽象工厂模式）
3. 设计模式的依赖抽象原则。
4. 创建对象实例时，不要直接new类，而是把这个new类的动作放在一个工厂的方法中，并返回。有些书上说，变量不要直接持有具体类的引用。
5. 不要让类继承具体类，而是继承抽象类或者实现interface（接口）
6. 不要覆盖基类中已经实现的方法。

<br>

### 原型模式

#### 传统方法

##### 克隆羊问题

现在有一只羊tom，姓名为：tom，年龄为：1，颜色为：白色，请编写程序创建和tom羊属性完全相同的10只羊。

##### 传统方式解决

思路分析：类图图解

![克隆羊类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-04-14%2009.59.55.png "克隆羊类图")

```java
public class Sheep {
	private String name;
	private int age;
	private String color;
	public Sheep(String name, int age, String color) {
		super();
		this.name = name;
		this.age = age;
		this.color = color;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
	@Override
	public String toString() {
		return "Sheep [name=" + name + ", age=" + age + ", color=" + color + "]";
	}
	
	
}





public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//传统的方法
		Sheep sheep = new Sheep("tom", 1, "白色");
		
		Sheep sheep2 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		Sheep sheep3 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		Sheep sheep4 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		Sheep sheep5 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		//....
		
		System.out.println(sheep);
		System.out.println(sheep2);
		System.out.println(sheep3);
		System.out.println(sheep4);
		System.out.println(sheep5);
		//...
	}

}
```

##### 传统方式的优缺点

1. 有点是比较好理解，简单易操作。
2. 在创建新的对象时，总是需要重新获取原始对象的属性，如果创建的对象比较复杂时，效率较低。
3. 总是需要重新初始化对象，而不是动态地获得对象运行时的状态，不够灵活。

{{< admonition example 改进思路与分析 >}}

思路：Java中Object类是所有类的根类，Object类提供了一个clone()方法，该方法可以将一个Java对象复制一份，但是需要实现clone的Java类必须要实现一个接口Cloneable，该接口表示该类能够复制且具有复制的能力->**原型模式**

{{< / admonition >}}

#### 原型模式

##### 介绍

1. 原型模式（Prototype模式）是指：用原型实例指定创建对象的种类，并且通过拷贝这些原型，创建新的对象。
2. 原型模式是一种创建型设计模式，允许一个对象再创建另外一个可定制的对象，无需知道如何创建的细节。
3. 工作原理：通过将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝它们自己来实施创建，即 对象.clone()
4. 形象的理解：孙大圣拔出猴毛，变出其他孙大圣。

##### 原型模式原理结构图-UML类图



![原型模式类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-04-14%2010.21.11.png "原型模式类图")



{{< admonition question 原理结构图说明 >}}

1. Prototype：原型类，声明一个克隆自己的接口
2. ConcretePrototype：具体的原型类，实现一个克隆自己的操作
3. Client：让一个原型对象克隆自己，从而创建一个新的对象（属性一样）

{{< /admonition >}}

##### 原型模式解决克隆羊问题

使用原型模式改进传统方式，让程序具有更高的效率和扩展性

```java
public class Sheep implements Cloneable {
	private String name;
	private int age;
	private String color;
	private String address = "蒙古羊";
	public Sheep friend; //是对象, 克隆是会如何处理
	public Sheep(String name, int age, String color) {
		super();
		this.name = name;
		this.age = age;
		this.color = color;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
	
	
	
	@Override
	public String toString() {
		return "Sheep [name=" + name + ", age=" + age + ", color=" + color + ", address=" + address + "]";
	}
	//克隆该实例，使用默认的clone方法来完成
	@Override
	protected Object clone()  {
		
		Sheep sheep = null;
		try {
			sheep = (Sheep)super.clone();
		} catch (Exception e) {
			// TODO: handle exception
			System.out.println(e.getMessage());
		}
		// TODO Auto-generated method stub
		return sheep;
	}
	
	
}




public class Client {

	public static void main(String[] args) {
		System.out.println("原型模式完成对象的创建");
		// TODO Auto-generated method stub
		Sheep sheep = new Sheep("tom", 1, "白色");
		
		sheep.friend = new Sheep("jack", 2, "黑色");
		
		Sheep sheep2 = (Sheep)sheep.clone(); //克隆
		Sheep sheep3 = (Sheep)sheep.clone(); //克隆
		Sheep sheep4 = (Sheep)sheep.clone(); //克隆
		Sheep sheep5 = (Sheep)sheep.clone(); //克隆
		
		System.out.println("sheep2 =" + sheep2 + "sheep2.friend=" + sheep2.friend.hashCode());
		System.out.println("sheep3 =" + sheep3 + "sheep3.friend=" + sheep3.friend.hashCode());
		System.out.println("sheep4 =" + sheep4 + "sheep4.friend=" + sheep4.friend.hashCode());
		System.out.println("sheep5 =" + sheep5 + "sheep5.friend=" + sheep5.friend.hashCode());
	}

}
```

#### 原型模式在Spring框架中源码分析

1. Spring中原型bean的创建，就是原型模式的应用
2. 代码分析

```java
/**
 * 注释
 * @author Administrator
 *
 */
public class Monster {

	private Integer id = 10 ;
	private String nickname = "牛魔王";
	private String skill = "芭蕉扇";
	public Monster() {
		
		System.out.println("monster 创建..");
	}
	public Monster(Integer id, String nickname, String skill) {
		//System.out.println("Integer id, String nickname, String skill被调用");
		this.id = id;
		this.nickname = nickname;
		this.skill = skill;
	}
	
	public Monster( String nickname, String skill,Integer id) {
		
		this.id = id;
		this.nickname = nickname;
		this.skill = skill;
	}
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getNickname() {
		return nickname;
	}
	public void setNickname(String nickname) {
		this.nickname = nickname;
	}
	public String getSkill() {
		return skill;
	}
	public void setSkill(String skill) {
		this.skill = skill;
	}
	@Override
	public String toString() {
		return "Monster [id=" + id + ", nickname=" + nickname + ", skill="
				+ skill + "]";
	}
	
	
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">


 <!-- 这里我们的 scope="prototype" 即 原型模式来创建 -->
 <bean id="id01" class="com.spring.bean.Monster" 
 	scope="prototype"/>
 
 
</beans>
```

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class ProtoType {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml");
		// 获取monster[通过id获取monster]
		Object bean = applicationContext.getBean("id01");
		System.out.println("bean" + bean); // 输出 "牛魔王" .....
		
		Object bean2 = applicationContext.getBean("id01");
		
		System.out.println("bean2" + bean2); //输出 "牛魔王" .....

		System.out.println(bean == bean2); // false
		
		// ConfigurableApplicationContext
	}

}
```

#### 深入讨论浅拷贝和深拷贝

##### 浅拷贝介绍

1. 对于数据类型是基本数据类型的成员变量，浅拷贝会直接进行值传递，也就是将该属性值复制一份给新的对象。
2. 对于数据类型是引用数据类型的成员变量，比如说成员变量是某个数组、某个类的对象等，那么浅拷贝会进行引用传递，也就是是只是将该成员变量的引用值（内存地址）复制一份给新的对象。因为实际上两个对象的该成员变量都指向同一个实例。在这种情况下，在一个对象中修改该成员变量会影响到另一个对象的该成员变量值。
3. 浅拷贝实现方式1:重写clone方法来实现浅拷贝
4. 浅拷贝实现方式2:通过对象序列化实现浅拷贝（**推荐**）

##### 浅拷贝应用实例

1. 使用重写clone方法实现浅拷贝
2. 使用序列化来实现浅拷贝
3. 代码演示

```java
import java.io.Serializable;

public class DeepCloneableTarget implements Serializable, Cloneable {
	
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;

	private String cloneName;

	private String cloneClass;

	//构造器
	public DeepCloneableTarget(String cloneName, String cloneClass) {
		this.cloneName = cloneName;
		this.cloneClass = cloneClass;
	}

	//因为该类的属性，都是String , 因此我们这里使用默认的clone完成即可
	@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
}







import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class DeepProtoType implements Serializable, Cloneable{
	
	public String name; //String 属性
	public DeepCloneableTarget deepCloneableTarget;// 引用类型
	public DeepProtoType() {
		super();
	}
	
	
	//深拷贝 - 方式 1 使用clone 方法
	@Override
	protected Object clone() throws CloneNotSupportedException {
		
		Object deep = null;
		//这里完成对基本数据类型(属性)和String的克隆
		deep = super.clone(); 
		//对引用类型的属性，进行单独处理
		DeepProtoType deepProtoType = (DeepProtoType)deep;
		deepProtoType.deepCloneableTarget  = (DeepCloneableTarget)deepCloneableTarget.clone();
		
		// TODO Auto-generated method stub
		return deepProtoType;
	}
	
	//深拷贝 - 方式2 通过对象的序列化实现 (推荐)
	
	public Object deepClone() {
		
		//创建流对象
		ByteArrayOutputStream bos = null;
		ObjectOutputStream oos = null;
		ByteArrayInputStream bis = null;
		ObjectInputStream ois = null;
		
		try {
			
			//序列化
			bos = new ByteArrayOutputStream();
			oos = new ObjectOutputStream(bos);
			oos.writeObject(this); //当前这个对象以对象流的方式输出
			
			//反序列化
			bis = new ByteArrayInputStream(bos.toByteArray());
			ois = new ObjectInputStream(bis);
			DeepProtoType copyObj = (DeepProtoType)ois.readObject();
			
			return copyObj;
			
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			return null;
		} finally {
			//关闭流
			try {
				bos.close();
				oos.close();
				bis.close();
				ois.close();
			} catch (Exception e2) {
				// TODO: handle exception
				System.out.println(e2.getMessage());
			}
		}
		
	}
	
}







public class Client {

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		DeepProtoType p = new DeepProtoType();
		p.name = "宋江";
		p.deepCloneableTarget = new DeepCloneableTarget("大牛", "小牛");
		
		//方式1 完成深拷贝
		
//		DeepProtoType p2 = (DeepProtoType) p.clone();
//		
//		System.out.println("p.name=" + p.name + "p.deepCloneableTarget=" + p.deepCloneableTarget.hashCode());
//		System.out.println("p2.name=" + p.name + "p2.deepCloneableTarget=" + p2.deepCloneableTarget.hashCode());
	
		//方式2 完成深拷贝
		DeepProtoType p2 = (DeepProtoType) p.deepClone();
		
		System.out.println("p.name=" + p.name + "p.deepCloneableTarget=" + p.deepCloneableTarget.hashCode());
		System.out.println("p2.name=" + p.name + "p2.deepCloneableTarget=" + p2.deepCloneableTarget.hashCode());
	
	}

}
```

##### 原型模式的注意事项和细节

{{< admonition tip 原型模式 >}}

1. 创建新的对象比较复杂时，可以利用原型模式简化对象的创建过程，同时也能够提高效率。
2. 不用重新初始化对象，而是动态地获得对象运行时的状态。
3. 如果原始对象发生变化（增加或者减少属性），其它克隆的对象也会发生相应的变化，无需修改代码。
4. 在实现深克隆的时候可能需要比较复杂的代码。
5. **缺点**：需要为每一个类配备一个克隆方法，这对全新的类来说不是很难，但对已有的类进行改造时，需要修改其源代码，这违背了OCP原则。

{{< /admonition >}}

<br>

### 建造者模式

#### 传统方法

##### 盖房问题

项目需求：

1. 需要建造房子：这一过程为打桩、砌墙、封顶
2. 房子有各种各样的，比如普通房，高楼，别墅，各种房子的过程虽然一样，但是要求不要相同的。

##### 传统方式解决

![传统方式分析类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-04-21%2010.13.32.png "传统方式分析类图")

```java
public abstract class AbstractHouse {
	
	//打地基
	public abstract void buildBasic();
	//砌墙
	public abstract void buildWalls();
	//封顶
	public abstract void roofed();
	
	public void build() {
		buildBasic();
		buildWalls();
		roofed();
	}
	
}




public class CommonHouse extends AbstractHouse {

	@Override
	public void buildBasic() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子打地基 ");
	}

	@Override
	public void buildWalls() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子砌墙 ");
	}

	@Override
	public void roofed() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子封顶 ");
	}

}



public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		CommonHouse commonHouse = new CommonHouse();
		commonHouse.build();
	}

}
```

##### 传统方式的问题分析

1. 优点是比较好理解，简单易操作。
2. 设计的程序结构，过于简单，没有设计缓存层对象，程序的扩展和维护不好，也就是说，这种设计方案，把产品（即：房子）和创建产品的过程（即：建房子流程）封装在一起，耦合性增强了。
3. 解决方案：将产品和产品建造过程耦合->建造者模式

#### 建造者模式基本介绍

1. **建造者模式**（Builder Pattern）又叫生成器模式，是一种对象构造模式。它可以将复杂对象的建造过程抽象出来（抽象类别），使这个抽象过程的不同实现方法可以构造出不同表现（属性）的对象。
2. **建造者模式 **是一步一步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构造他们，用户不需要知道内部的具体构建细节。

{{< admonition question 建造者模式的四个角色 >}}

1. **Product**（**产品角色**）：一个具体的产品对象
2. **Builder**（**抽象建造者**）：创建一个Product对象的各个部件指定的接口/抽象类。
3. **ConcreteBuilder**（具体建造者）：实现接口，构造和装配各个部件。
4. **Director**（**指挥者**）：构建一个使用Builder接口的对象。它主要是用于创建一个复杂的对象。它主要有两个作用，一：隔离了客户与对象的生产过程，二：负责控制产品对象的生产过程。

{{< /admonition >}}

#### 建造者模式原理类图

![建造者模式类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-04-21%2010.40.20.png "建造者模式类图")

#### 建造者模式解决盖房问题

![盖房问题类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-04-21%2010.42.44.png "盖房问题类图")

```java
// 抽象的建造者
public abstract class HouseBuilder {

	protected House house = new House();
	
	//将建造的流程写好, 抽象的方法
	public abstract void buildBasic();
	public abstract void buildWalls();
	public abstract void roofed();
	
	//建造房子好， 将产品(房子) 返回
	public House buildHouse() {
		return house;
	}
	
}




//指挥者，这里去指定制作流程，返回产品
public class HouseDirector {
	
	HouseBuilder houseBuilder = null;

	//构造器传入 houseBuilder
	public HouseDirector(HouseBuilder houseBuilder) {
		this.houseBuilder = houseBuilder;
	}

	//通过setter 传入 houseBuilder
	public void setHouseBuilder(HouseBuilder houseBuilder) {
		this.houseBuilder = houseBuilder;
	}
	
	//如何处理建造房子的流程，交给指挥者
	public House constructHouse() {
		houseBuilder.buildBasic();
		houseBuilder.buildWalls();
		houseBuilder.roofed();
		return houseBuilder.buildHouse();
	}
	
	
}



//产品->Product
public class House {
	private String baise;
	private String wall;
	private String roofed;
	public String getBaise() {
		return baise;
	}
	public void setBaise(String baise) {
		this.baise = baise;
	}
	public String getWall() {
		return wall;
	}
	public void setWall(String wall) {
		this.wall = wall;
	}
	public String getRoofed() {
		return roofed;
	}
	public void setRoofed(String roofed) {
		this.roofed = roofed;
	}
	
}




public class HighBuilding extends HouseBuilder {

	@Override
	public void buildBasic() {
		// TODO Auto-generated method stub
		System.out.println(" 高楼的打地基100米 ");
	}

	@Override
	public void buildWalls() {
		// TODO Auto-generated method stub
		System.out.println(" 高楼的砌墙20cm ");
	}

	@Override
	public void roofed() {
		// TODO Auto-generated method stub
		System.out.println(" 高楼的透明屋顶 ");
	}

}




public class CommonHouse extends HouseBuilder {

	@Override
	public void buildBasic() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子打地基5米 ");
	}

	@Override
	public void buildWalls() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子砌墙10cm ");
	}

	@Override
	public void roofed() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子屋顶 ");
	}

}



public class Client {
	public static void main(String[] args) {
		
		//盖普通房子
		CommonHouse commonHouse = new CommonHouse();
		//准备创建房子的指挥者
		HouseDirector houseDirector = new HouseDirector(commonHouse);
		
		//完成盖房子，返回产品(普通房子)
		House house = houseDirector.constructHouse();
		
		//System.out.println("输出流程");
		
		System.out.println("--------------------------");
		//盖高楼
		HighBuilding highBuilding = new HighBuilding();
		//重置建造者
		houseDirector.setHouseBuilder(highBuilding);
		//完成盖房子，返回产品(高楼)
		houseDirector.constructHouse();	
		
	}
}

```

#### 建造者模式在JDK的应用和源码分析

**java.lang.StringBuilder中的建造者模式**

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    char[] value;

    /**
     * The count is the number of characters used.
     */
    int count;
  
  /**
     * Appends the specified string to this character sequence.
     * <p>
     * The characters of the {@code String} argument are appended, in
     * order, increasing the length of this sequence by the length of the
     * argument. If {@code str} is {@code null}, then the four
     * characters {@code "null"} are appended.
     * <p>
     * Let <i>n</i> be the length of this character sequence just prior to
     * execution of the {@code append} method. Then the character at
     * index <i>k</i> in the new character sequence is equal to the character
     * at index <i>k</i> in the old character sequence, if <i>k</i> is less
     * than <i>n</i>; otherwise, it is equal to the character at index
     * <i>k-n</i> in the argument {@code str}.
     *
     * @param   str   a string.
     * @return  a reference to this object.
     */
  public AbstractStringBuilder append(String str) {
        if (str == null)
            return appendNull();
        int len = str.length();
        ensureCapacityInternal(count + len);
        str.getChars(0, len, value, count);
        count += len;
        return this;
    }
}





public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
{

    /** use serialVersionUID for interoperability */
    static final long serialVersionUID = 4383685877147921099L;

    @Override
    public StringBuilder append(String str) {
        super.append(str);
        return this;
    }
}
```

{{< admonition question 源码中建造者模式角色分析 >}}

1. **Appendable**接口定义了多个append方法（**抽象方法**），即Appendable为**抽象建造者**，定义了抽象方法
2. **AbstractStringBuilder**实现了Appendable接口方法，这里的AbstractStringBuilder已经是**建造者**，只是不能实例化。
3. **StringBuilder**即充当了**指挥者**角色，同时充当了具体的**建造者**，建造方法的实现是由AbstractStringBuilder完成，而StringBuilder继承了AbstractStringBuilder。

{{< /admonition >}}

#### 建造者模式的注意事项和细节

{{< admonition tip 建造者模式 >}}

1. 客户端（使用程序）**不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦**，使得相同的建造过程可以创建不同的产品对象。
2. **每一个具体创造者都相对独立，而与其他的具体建造者无关**，因此可以很方便地替换具体建造者或增加新的具体建造者，用户使用不同的具体建造者即可得到不同的产品对象。
3. 可以更加精细地控制产品的创建过程。将复杂产品的创建步骤分解在不同的方法中，使得建造过程更加清晰，也更方便使用程序来控制创建过程
4. 增加新的具体建造者无需修改原有类库的代码，指挥者类针对抽象建造者类编程，系统扩展方便，符合“开闭原则”。
5. 建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，**如果产品之间差异性很大，则不适合使用建造者模式**，因此其使用范围受到一定的限制。
6. 如果产品内部变化复杂可能会导致需要定义很多具体建造者来实现这种变化，导致系统变得很庞大，因此在这种情况下，要考虑是是否选择建造者模式。
7. **抽象工厂模式 VS 建造者模式**

抽象工厂模式实现对产品家族的创建，一个产品家族是这样的一系列产品：具有不同分类维度的产品组合，采用抽象工厂模式**不需要关心构建过程**，只关心什么产品由什么工厂生产即可。而建造者模式则是**要求按照指定的蓝图建造产品**，它的主要目的是通过组装零配件而产生一个新的产品。

{{< /admonition >}}

<br>

### 适配器模式

#### 介绍

1. 适配器模式（Adapter Pattern）将某个类的接口转换成客户端期望的另一个接口表示，主要目的是兼容性，让原本因接口不匹配不能一起工作的两个类可以协同工作。其别名为**包装器（Wrapper）**
2. 适配器模式属于**结构型模式**
3. 主要分为三类：**类适配器模式、对象适配器模式、接口适配器模式**

#### 适配器模式工作原理

1. 适配器模式：将一个类的接口转换成另一种接口，让原本接口不兼容的类可以兼容
2. 从**用户的角度看不到适配者**，是解耦的
3. 用户调用适配器转化出来的目标接口方法，适配器再调用被适配者的相关接口方法
4. 用户用收到反馈结果，感觉只是和目标交互

#### 类适配器模式

##### 介绍

Adapter类，通过继承src类，实现dst类接口，完成 src -> dst 的适配

##### 应用实例

生活中充电器的例子来反应适配器模式，充电器本身相当于Adapter，220V交流电相当于src（即被适配者），我们的dst（即目标)是5V直流电

![类适配器实例](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-05%2010.21.27.png "类适配器实例")

```java
//适配接口
public interface IVoltage5V {
	public int output5V();
}



//被适配的类
public class Voltage220V {
	//输出220V的电压
	public int output220V() {
		int src = 220;
		System.out.println("电压=" + src + "伏");
		return src;
	}
}



//适配器类
public class VoltageAdapter extends Voltage220V implements IVoltage5V {

	@Override
	public int output5V() {
		// TODO Auto-generated method stub
		//获取到220V电压
		int srcV = output220V();
		int dstV = srcV / 44 ; //转成 5v
		return dstV;
	}

}



public class Phone {

	//充电
	public void charging(IVoltage5V iVoltage5V) {
		if(iVoltage5V.output5V() == 5) {
			System.out.println("电压为5V, 可以充电~~");
		} else if (iVoltage5V.output5V() > 5) {
			System.out.println("电压大于5V, 不能充电~~");
		}
	}
}



public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println(" === 类适配器模式 ====");
		Phone phone = new Phone();
		phone.charging(new VoltageAdapter());
	}

}

```

##### 类适配器模式注意事项和细节

{{< admonition 类适配器模式 >}}

1. Java是单继承机制，所以类适配器需要继承src类这一点算是一个缺点，因为这要求dst必须是接口，有一定局限性
2. src类的方法在Adapter中都会暴露出来，也增加了使用的成本
3. 由于其继承了src类，所以它可以根据需求重写src类的方法，使得Adapter的灵活性增强了。

{{< /admonition >}}

<br>

#### 对象适配器模式

##### 介绍

1. 基本思路和类的适配器模式相同，只是将Adapter类作修改，不是继承src类，而是持有src类的实例，以解决兼容性的问题。即：持有src类，实现dst类接口，完成 src -> dst 的适配
2. 根据“**合成复用原则**”，在系统中**尽量使用关联关系来代替继承关系**
3. 对象适配器模式是适配器模式常用的一种

##### 应用实例

生活中充电器的例子来讲解适配器，充电器本身相当于Adapter，220V交流电相当于src（即被适配者），我们的dst（即目标）是5V直流电，使用**对象适配器模式**完成

![对象适配器实例](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%AF%B9%E8%B1%A1%E9%80%82%E9%85%8D%E5%99%A8%E5%AE%9E%E4%BE%8B.png ""对象适配器实例"")

```java
//适配接口
public interface IVoltage5V {
	public int output5V();
}




//被适配的类
public class Voltage220V {
	//输出220V的电压，不变
	public int output220V() {
		int src = 220;
		System.out.println("电压=" + src + "伏");
		return src;
	}
}




//适配器类
public class VoltageAdapter  implements IVoltage5V {

	private Voltage220V voltage220V; // 关联关系-聚合
	
	
	//通过构造器，传入一个 Voltage220V 实例
	public VoltageAdapter(Voltage220V voltage220v) {
		
		this.voltage220V = voltage220v;
	}

	@Override
	public int output5V() {
		
		int dst = 0;
		if(null != voltage220V) {
			int src = voltage220V.output220V();//获取220V 电压
			System.out.println("使用对象适配器，进行适配~~");
			dst = src / 44;
			System.out.println("适配完成，输出的电压为=" + dst);
		}
		
		return dst;
		
	}

}




public class Phone {

	//充电
	public void charging(IVoltage5V iVoltage5V) {
		if(iVoltage5V.output5V() == 5) {
			System.out.println("电压为5V, 可以充电~~");
		} else if (iVoltage5V.output5V() > 5) {
			System.out.println("电压大于5V, 不能充电~~");
		}
	}
}




public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println(" === 对象适配器模式 ====");
		Phone phone = new Phone();
		phone.charging(new VoltageAdapter(new Voltage220V()));
	}

}
```

##### 对象适配器模式注意事项和细节

{{< admonition tip 对象适配器 >}}

1. 对象适配器和类适配器其实算是同一种思想，只不过实现方式不同。根据合成复用原则，使用组合替代继承，所以它解决了类适配器必须继承src的局限性问题，也不再要求dst必须是接口
2. 使用成本更低，更灵活

{{< /admonition >}}

<br>

#### 接口适配器模式

##### 介绍

1. 一些书籍称为：适配器模式（Default Adapter Pattern）或缺省适配器模式
2. 当不需要全部实现接口提供的方法时，可以先设计一个抽象类实现接口，并为该接口中每个方法提供一个默认实现（空方法），那么该抽象类的字类可有选择地覆盖父类的某些方法来实现需求
3. 适用于一个接口不想使用其所有的方法的情况

##### 应用实例

![接口适配器实例](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-05%2008.31.49.png "接口适配器实例")

```java
public interface InterfaceAdapter {
	public void m1();
	public void m2();
	public void m3();
	public void m4();
}



//在AbsAdapter 我们将 Interface4 的方法进行默认实现
public abstract class AbsAdapter implements InterfaceAdapter {

	//默认实现
	public void m1() {

	}

	public void m2() {

	}

	public void m3() {

	}

	public void m4() {

	}
}



public class Client {
	public static void main(String[] args) {
		
		AbsAdapter absAdapter = new AbsAdapter() {
			//只需要去覆盖我们 需要使用 接口方法
			@Override
			public void m1() {
				// TODO Auto-generated method stub
				System.out.println("使用了m1的方法");
			}
		};
		
		absAdapter.m1();
	}
}
```

有时候我们不想实现**InterfaceAdapter接口**中的全部方法，我们只想监听m1方法，我们只需按如上方式实现**接口适配器**。

AbsAdapter类，就是一个**接口适配器**，它空实现了InterfaceAdapter接口中的所有方法。

#### 适配器模式在SpringMVC框架应用的源码分析

1. SpringMVC中的**HandleAdapter**，就使用了适配器模式
2. 使用HandleAdapter的原因分析：

根据源码分析可以看到处理器的类型不同，有**多重实现方式**，那么调用方式就是不确定的，如果需要直接调用Controller方法，需要调用的时候就得不断使用if-else来进行判断是哪一种自类然后执行。那么如果后面要扩展Controller，就得修改原来的代码，这样就违背了OCP原则。

```java
//多种Controller实现  
public interface Controller {

}

class HttpController implements Controller {
	public void doHttpHandler() {
		System.out.println("http...");
	}
}

class SimpleController implements Controller {
	public void doSimplerHandler() {
		System.out.println("simple...");
	}
}

class AnnotationController implements Controller {
	public void doAnnotationHandler() {
		System.out.println("annotation...");
	}
}




import java.util.ArrayList;
import java.util.List;

public class DispatchServlet {

	public static List<HandlerAdapter> handlerAdapters = new ArrayList<HandlerAdapter>();

	public DispatchServlet() {
		handlerAdapters.add(new AnnotationHandlerAdapter());
		handlerAdapters.add(new HttpHandlerAdapter());
		handlerAdapters.add(new SimpleHandlerAdapter());
	}

	public void doDispatch() {

		// 此处模拟SpringMVC从request取handler的对象，
		// 适配器可以获取到希望的Controller
		 HttpController controller = new HttpController();
		// AnnotationController controller = new AnnotationController();
		//SimpleController controller = new SimpleController();
		// 得到对应适配器
		HandlerAdapter adapter = getHandler(controller);
		// 通过适配器执行对应的controller对应方法
		adapter.handle(controller);

	}

	public HandlerAdapter getHandler(Controller controller) {
		//遍历：根据得到的controller(handler), 返回对应适配器
		for (HandlerAdapter adapter : this.handlerAdapters) {
			if (adapter.supports(controller)) {
				return adapter;
			}
		}
		return null;
	}

	public static void main(String[] args) {
		new DispatchServlet().doDispatch(); // http...
	}

}




///定义一个Adapter接口 
public interface HandlerAdapter {
	public boolean supports(Object handler);

	public void handle(Object handler);
}

// 多种适配器类

class SimpleHandlerAdapter implements HandlerAdapter {

	public void handle(Object handler) {
		((SimpleController) handler).doSimplerHandler();
	}

	public boolean supports(Object handler) {
		return (handler instanceof SimpleController);
	}

}

class HttpHandlerAdapter implements HandlerAdapter {

	public void handle(Object handler) {
		((HttpController) handler).doHttpHandler();
	}

	public boolean supports(Object handler) {
		return (handler instanceof HttpController);
	}

}

class AnnotationHandlerAdapter implements HandlerAdapter {

	public void handle(Object handler) {
		((AnnotationController) handler).doAnnotationHandler();
	}

	public boolean supports(Object handler) {

		return (handler instanceof AnnotationController);
	}

}
```

**说明**

- Spring定义了一个适配器接口，使得每一种Controller有一种对应的适配器实现类
- 适配器代替了Controller执行相应的方法
- 扩展Controller时，只需要增加一个适配器类就完成了SpringMVC的扩展。

#### 适配器模式的注意事项和细节

{{< admonition tip 适配器模式 >}}

1. 三种命名方式，是根据src是以怎样的形式给到Adapter（在Adapter里的形式）来命名的
2. **类适配器**：以类给到，在Adapter里，就是将src当作类，继承
3. **对象适配器**：给对象到，在Adapter里，将src作为一个对象，持有
4. **接口适配器**：以接口给到，在Apapter里，将src作为一个接口，实现
5. Adapter模式最大的作用还是将原本不兼容的接口融合在一起工作
6. 实际开发中，实现起来不拘泥于我们讲解的三种惊呆你形式

{{< /admonition >}}

<br>

### 桥接模式

#### 传统方法

##### 手机操作问题

现在对于不同手机类型的不同品牌实现操作编程（比如：开机、关机、上网、打电话等），如下图所示：

![手机类型分类](https://cdn.jsdelivr.net/gh/Turbo-King/images/phone.png "Phone")

##### 传统方案解决手机使用问题

传统方法对应的类图

![Phone类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/Phone%E7%B1%BB%E5%9B%BE.png "手机使用问题类图")

##### 传统方案解决手机操作类型

1. 扩展性问题（类爆炸），如果我们再增加手机的样式（旋转式）,就需要增加各个品牌手机的类，同样如果我们增加一个手机品牌，也要在各个手机样式类下增加
2. 违反了单一职责原则，当我们增加手机样式时，要同时增加所有品牌的手机，这样增加了代码维护成本
3. 解决方案-使用桥接模式

#### 桥接模式

##### 基本介绍

1. **桥接模式（Bridge模式）**是指：将实现与抽象放在两个不同的类层次中，使两个层次可以独立改变
2. 是一种**结构型设计模式**
3. Bridge模式基于**类的最小设计原则**，通过使用**封装**、**聚合**及**继承**等行为让不同的类承担不同的职责。它的主要特点是把**抽象（Abstraction）与行为实现（Implementation）分离开来**，从而可以保持各部分的独立性以及应对他们的功能扩展

##### 桥接模式原理类图

![桥接模式类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-05%2011.19.25.png "桥接模式类图")

**类图说明**

1. Client类：桥接模式模式的调用者
2. 抽象类（Abstraction）：维护了Implementor/即它的实现类Concrete ImplementorA..，二者是聚合关系，Abstraction充当桥接类
3. RefinedAbstraction：是Abstraction抽象类的字类
4. Implementor：行为实现类的接口
5. Concrete ImplementorA/B：行为的具体实现类
6. 从UML图：这里的抽象类和接口是聚合的关系，其实调用和被调用关系

##### 桥接模式解决手机操作问题

使用桥接模式改进传统方式，让程序具有搞好的扩展性，利用程序维护

![桥接模式解决手机操作问题类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E6%89%8B%E6%9C%BA%E6%93%8D%E4%BD%9C%E9%97%AE%E9%A2%98%E7%B1%BB%E5%9B%BE.png "桥接模式解决手机操作问题类图")

```java
//接口
public interface Brand {
	void open();
	void close();
	void call();
}



public abstract class Phone {
	
	//组合品牌
	private Brand brand;

	//构造器
	public Phone(Brand brand) {
		super();
		this.brand = brand;
	}
	
	protected void open() {
		this.brand.open();
	}
	protected void close() {
		brand.close();
	}
	protected void call() {
		brand.call();
	}
	
}



//折叠式手机类，继承 抽象类 Phone
public class FoldedPhone extends Phone {

	//构造器
	public FoldedPhone(Brand brand) {
		super(brand);
	}
	
	public void open() {
		super.open();
		System.out.println(" 折叠样式手机 ");
	}
	
	public void close() {
		super.close();
		System.out.println(" 折叠样式手机 ");
	}
	
	public void call() {
		super.call();
		System.out.println(" 折叠样式手机 ");
	}
}



public class UpRightPhone extends Phone {
	
		//构造器
		public UpRightPhone(Brand brand) {
			super(brand);
		}
		
		public void open() {
			super.open();
			System.out.println(" 直立样式手机 ");
		}
		
		public void close() {
			super.close();
			System.out.println(" 直立样式手机 ");
		}
		
		public void call() {
			super.call();
			System.out.println(" 直立样式手机 ");
		}
}



public class Vivo implements Brand {

	@Override
	public void open() {
		// TODO Auto-generated method stub
		System.out.println(" Vivo手机开机 ");
	}

	@Override
	public void close() {
		// TODO Auto-generated method stub
		System.out.println(" Vivo手机关机 ");
	}

	@Override
	public void call() {
		// TODO Auto-generated method stub
		System.out.println(" Vivo手机打电话 ");
	}

}



public class XiaoMi implements Brand {

	@Override
	public void open() {
		// TODO Auto-generated method stub
		System.out.println(" 小米手机开机 ");
	}

	@Override
	public void close() {
		// TODO Auto-generated method stub
		System.out.println(" 小米手机关机 ");
	}

	@Override
	public void call() {
		// TODO Auto-generated method stub
		System.out.println(" 小米手机打电话 ");
	}

}



public class Client {

	public static void main(String[] args) {
		
		//获取折叠式手机 (样式 + 品牌 )
		
		Phone phone1 = new FoldedPhone(new XiaoMi());
		
		phone1.open();
		phone1.call();
		phone1.close();
		
		System.out.println("=======================");
		
		Phone phone2 = new FoldedPhone(new Vivo());
		
		phone2.open();
		phone2.call();
		phone2.close();
		
		System.out.println("==============");
		
		UpRightPhone phone3 = new UpRightPhone(new XiaoMi());
		
		phone3.open();
		phone3.call();
		phone3.close();
		
		System.out.println("==============");
		
		UpRightPhone phone4 = new UpRightPhone(new Vivo());
		
		phone4.open();
		phone4.call();
		phone4.close();
	}

}
```

#### 桥接模式在JDBC的源码剖析

JDBC的Driver接口，如果从**桥接模式**来看，**Driver**就是一个接口，下面可以有MySQL的Driver，Oracle的Driver，这些就可以当作实现接口类

![JDBC源码分析类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-09%2010.46.43.png "JDBC源码分析类图")

```java
import java.sql.SQLException;

/**
 * The Java SQL framework allows for multiple database drivers. Each driver should supply a class that implements the Driver interface
 * 
 * <p>
 * The DriverManager will try to load as many drivers as it can find and then for any given connection request, it will ask each driver in turn to try to
 * connect to the target URL.
 * 
 * <p>
 * It is strongly recommended that each Driver class should be small and standalone so that the Driver class can be loaded and queried without bringing in vast
 * quantities of supporting code.
 * 
 * <p>
 * When a Driver class is loaded, it should create an instance of itself and register it with the DriverManager. This means that a user can load and register a
 * driver by doing Class.forName("foo.bah.Driver")
 */
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    //
    // Register ourselves with the DriverManager
    //
    static {
        try {
            java.sql.DriverManager.registerDriver(new Driver());
        } catch (SQLException E) {
            throw new RuntimeException("Can't register driver!");
        }
    }

    /**
     * Construct a new driver and register it with DriverManager
     * 
     * @throws SQLException
     *             if a database error occurs.
     */
    public Driver() throws SQLException {
        // Required for Class.forName().newInstance()
    }
}
```

**说明：**

1. MySQL有自己的ConnectuinImpl类，同样Oracle也有对应的实现类
2. Driver和Connection之间是通过DriverManager类进行桥连接的

#### 桥接模式的注意事项和细节

{{< admonition tip 桥接模式 >}}

1. 实现了抽象和实现部分的分离，从而极大的提供了系统的灵活性，让抽象部分和实现部分独立开来，这有助于系统进行分层设计，从而产生更好的结构化系统。
2. 对于系统的高层部分，只需知道抽象部分和实现部分的接口就可以了，其它的部分由具体业务来完成。
3. 桥接模式替代多层继承方案，可以减少字类的个数，降低系统的管理和维护成本。
4. 桥接模式的引入增加了系统的理解和设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计和编程。
5. 桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围有一定的局限性，既需要有这样的应用场景。

{{< /admonition >}}

#### 桥接模式其他应用场景

1. 对于那些不希望使用继承或因为多层继承导致系统类的个数急剧增加的系统，桥接模式尤为适用
2. 常见具体场景：

- JDBC驱动程序
- 银行转账系统
    - 转账分类：网上转账，柜台转账，ATM转账
    - 转账用户类型：普通用户，银卡用户，金卡用户
- 消息管理
    - 消息类型：即时消息，延时消息
    - 消息分类：手机短信，邮件消息，QQ消息

<br>

### 装饰者模式

#### 传统方法

##### 咖啡订单项目（咖啡馆）

1. 咖啡种类/单品咖啡：Espresso（意大利浓缩咖啡）、ShortBlack、LongBlack（美式咖啡）、Decaf（无因咖啡）
2. 调料：Milk、Soy（豆浆）、Chocolate
3. 要求在扩展**新的咖啡种类**时，具有良好的扩展性、改动方便、维护方便
4. 使用OO的来计算不同种类咖啡的**费用**：客户可以点**单品咖啡**、也可以点**单品咖啡+调料组合**

##### 解决方案一

![咖啡订单解决方案一](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-09%2011.34.39-20230509113642796.png "咖啡订单解决方案一")

**方案一问题分析**

1. Drink是一个抽象类，表示饮料
2. des就是对咖啡的描述，比如咖啡的名字
3. cost()方法就是计算费用，Drink类中做一个抽象方法
4. Decaf就是单品咖啡，继承Drink，并实现cost方法
5. Espress && Milk 就是单品咖啡+调料，这个组合很多

**问题：这样设计会有很多类，当我们增加一个单品咖啡或者一个新的调料，类的数量就会倍增，就会出现类爆炸**

##### 解决方案二

前面分析到方案一因为咖啡单品+调料组合会造成类的倍增，因此可以做改进，将调料内置到Drink类，这样就不会造成类数量过多。从而提高项目的维护性（如下图）

![咖啡馆解决方案二](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%92%96%E5%95%A1%E9%A6%86%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%BA%8C.png "咖啡馆解决方案二")

**说明：**milk，soy，chocolate 可以设计为Boolean，表示是否要添加相应的调料

**方案二分析**

1. 方案二可以控制类的数量，不至于造成很多的类
2. 在增加或者删除调料种类时，代码的维护量很大
3. 考虑到用户可以添加多份调料时，可以将hasMIlk返回一个对应的int
4. 考虑使用**装饰者模式**

#### 装饰者模式

##### 基本介绍

1. **装饰者模式**：动态的将新功能附加到对象上。在对象功能扩展方面，它比继承更有弹性，**装饰者模式也体现了开闭原则（OCP）**
2. 这里提到的动态的将新功能附加到对象和OCP原则，在后面的应用实例上会以代码的形式体现，请读者注意。

##### 装饰者模式原理

![原理图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2008.55.43.png "原理图")

1. 装饰者模式就像打包一个快递

主体：比如：陶瓷、衣服（Component） //被装饰者

包装：比如：报纸填充、塑料泡沫、纸板、木板（Decorator）

2. Component主体：比如类似前面的Drink
3. ConcreteComponent：具体的主体，比如前面各个单品咖啡
4. Decorator：装饰者，比如各种调料

如图中Component与ConcreteComponent之间，如果ConcreteComponent类很多，还可以设计一个缓冲层，将共有的部分提取出来，抽象层一个类

##### 装饰者模式解决咖啡订单

![装饰者模式解决咖啡订单](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2009.00.52.png "装饰者模式解决咖啡订单")

**说明：**

1. Drink 类就是前面说的抽象类，Component
2. ShortBlack 就是单品咖啡
3. Decorator是一个装饰类，含有一个被装饰的对象（Drink obj）
4. Decorator的cost方法进行一个费用的叠加计算，递归的计算价格

装饰者模式下的订单：2份巧克力+1份牛奶的LongBlack

![装饰者模式下的订单](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2009.04.56.png "装饰者模式下的订单")

**说明：**

1. Milk包含了LongBlack
2. 一份Chocolate包含了（Milk+LongBlack）
3. 一份Chocolate包含了（Chocolate+Milk+LongBlack）
4. 这样是不管什么形式的单品咖啡+调料组合，通过递归方式可以方便的组合和维护

##### 装饰者模式咖啡订单应用实例

```java
public abstract class Drink {

	public String des; // 描述
	private float price = 0.0f;
	public String getDes() {
		return des;
	}
	public void setDes(String des) {
		this.des = des;
	}
	public float getPrice() {
		return price;
	}
	public void setPrice(float price) {
		this.price = price;
	}
	
	//计算费用的抽象方法
	//子类来实现
	public abstract float cost();
	
}




public class Coffee  extends Drink {

	@Override
	public float cost() {
		// TODO Auto-generated method stub
		return super.getPrice();
	}

	
}




public class DeCaf extends Coffee {

	public DeCaf() {
		setDes(" 无因咖啡 ");
		setPrice(1.0f);
	}
}




public class Decorator extends Drink {
	private Drink obj;
	
	public Decorator(Drink obj) { //组合
		// TODO Auto-generated constructor stub
		this.obj = obj;
	}
	
	@Override
	public float cost() {
		// TODO Auto-generated method stub
		// getPrice 自己价格
		return super.getPrice() + obj.cost();
	}
	
	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		// obj.getDes() 输出被装饰者的信息
		return des + " " + getPrice() + " && " + obj.getDes();
	}
	
	

}




public class Espresso extends Coffee {
	
	public Espresso() {
		setDes(" 意大利咖啡 ");
		setPrice(6.0f);
	}
}




//具体的Decorator， 这里就是调味品
public class Chocolate extends Decorator {

	public Chocolate(Drink obj) {
		super(obj);
		setDes(" 巧克力 ");
		setPrice(3.0f); // 调味品 的价格
	}

}




public class LongBlack extends Coffee {

	public LongBlack() {
		setDes(" longblack ");
		setPrice(5.0f);
	}
}



public class ShortBlack extends Coffee{
	
	public ShortBlack() {
		setDes(" shortblack ");
		setPrice(4.0f);
	}
}




public class Milk extends Decorator {

	public Milk(Drink obj) {
		super(obj);
		// TODO Auto-generated constructor stub
		setDes(" 牛奶 ");
		setPrice(2.0f); 
	}

}



public class Soy extends Decorator{

	public Soy(Drink obj) {
		super(obj);
		// TODO Auto-generated constructor stub
		setDes(" Soy  ");
		setPrice(1.5f);
	}

}




public class CoffeeBar {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// 装饰者模式下的订单：2份巧克力+一份牛奶的LongBlack

		// 1. 点一份 LongBlack
		Drink order = new LongBlack();
		System.out.println("费用1=" + order.cost());
		System.out.println("描述=" + order.getDes());

		// 2. order 加入一份牛奶
		order = new Milk(order);

		System.out.println("order 加入一份牛奶 费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 描述 = " + order.getDes());

		// 3. order 加入一份巧克力

		order = new Chocolate(order);

		System.out.println("order 加入一份牛奶 加入一份巧克力  费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 加入一份巧克力 描述 = " + order.getDes());

		// 3. order 加入一份巧克力

		order = new Chocolate(order);

		System.out.println("order 加入一份牛奶 加入2份巧克力   费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 加入2份巧克力 描述 = " + order.getDes());
	
		System.out.println("===========================");
		
		Drink order2 = new DeCaf();
		
		System.out.println("order2 无因咖啡  费用 =" + order2.cost());
		System.out.println("order2 无因咖啡 描述 = " + order2.getDes());
		
		order2 = new Milk(order2);
		
		System.out.println("order2 无因咖啡 加入一份牛奶  费用 =" + order2.cost());
		System.out.println("order2 无因咖啡 加入一份牛奶 描述 = " + order2.getDes());

	
	}

}

```

#### 装饰者模式在JDK应用的源码分析

**Java的IO结构，FileInputStream就是一个装饰者**

![FileInputStream](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2009.32.02.png "FileInputStream")

```java
import java.io.DataInputStream;
import java.io.FileInputStream;
import java.io.InputStream;

public class Decorator {

	public static void main(String[] args) throws Exception{
		// TODO Auto-generated method stub
		
		//说明
		//1. InputStream 是抽象类, 类似我们前面讲的 Drink
		//2. FileInputStream 是  InputStream 子类，类似我们前面的 DeCaf, LongBlack
		//3. FilterInputStream  是  InputStream 子类：类似我们前面 的 Decorator 修饰者
		//4. DataInputStream 是 FilterInputStream 子类，具体的修饰者，类似前面的 Milk, Soy 等
		//5. FilterInputStream 类 有  protected volatile InputStream in; 即含被装饰者
		//6. 分析得出在jdk 的io体系中，就是使用装饰者模式
		
		DataInputStream dis = new DataInputStream(new FileInputStream("d:\\abc.txt"));
		System.out.println(dis.read());
		dis.close();
	}

}
```

### 组合模式

#### 传统方法

##### 学校院系展示需求

编写程序展示一个学校院系结构：需求是这样的，要在一个页面中展示出学校的院系组成，一个学校有多个学院，一个学院有多个系。如下图所示。

![学院展示需求](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2010.22.41.png "学院展示需求")

##### 传统方案解决学校院系展示（类图）

![传统解决方案类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BC%A0%E7%BB%9F%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E7%B1%BB%E5%9B%BE.png "传统解决方案类图")

##### 传统方案解决学校院系展示存在的问题分析

1. 将学院看作是学校的子类，系是学院的子类，这样实际上是站在组织大小来进行分层次的
2. 实际上我们的要求是：在一个页面中展示出学校的院系组成，一个学校有多个学院，一个学院有多个院系，因此这种方案，不能很好实现的管理操作，比如对学院、系的添加，删除，遍历等
3. 解决方案：把学校、院、系都看做是组织结构，他们之间没有继承的关系，而是一个树形结构，可以更好的实现管理操作->(组合模式)

#### 组合模式

##### 基本介绍

1. 组合模式（Composite Pattern），又叫部分整体模式，它创建了对象组的树形结构，将对象组合成树状结构以表示“整体-部分”的层次关系
2. 组合模式依据树形结构来组合对象，用来表示部分以及整体层次
3. 这种类型的设计模式属于结构型模式
4. 组合模式使得用户对单个对象和组合对象的访问具有一致性，即：组合能让客户以一致的方式处理个别对象以及组合对象

##### 组合模式的原理类图

![组合模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2010.44.00.png "组合模式原理类图")

{{< admonition question 组合模式的角色及职责 >}}

1. Component：这是组合中对象声明接口，在适当情况下，实现所有类共有的接口默认行为，用于访问和管理Component子部件，Component可以是抽象类或者接口
2. Leaf：在组合中表示叶子节点，叶子节点没有子节点
3. Composite：非叶子节点，用于存储子部件，在Component接口中实现子部件的相关操作，比如增加（add），删除（delete）

{{< /admonition >}}

##### 组合模式解决学校院系展示的应用实例

需求：编写程序展示一个学校院系结构，要在一个页面中展示出学校的院系组成，一个学校有多个学院，一个学院有多个系

![组合模式解决学校院系展示](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-12%2010.59.50.png "组合模式解决学校院系展示")

```java
public abstract class OrganizationComponent {

	private String name; // 名字
	private String des; // 说明
	
	protected  void add(OrganizationComponent organizationComponent) {
		//默认实现
		throw new UnsupportedOperationException();
	}
	
	protected  void remove(OrganizationComponent organizationComponent) {
		//默认实现
		throw new UnsupportedOperationException();
	}

	//构造器
	public OrganizationComponent(String name, String des) {
		super();
		this.name = name;
		this.des = des;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDes() {
		return des;
	}

	public void setDes(String des) {
		this.des = des;
	}
	
	//方法print, 做成抽象的, 子类都需要实现
	protected abstract void print();
	
	
}





import java.util.ArrayList;
import java.util.List;

//University 就是 Composite , 可以管理College
public class University extends OrganizationComponent {

	List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

	// 构造器
	public University(String name, String des) {
		super(name, des);
		// TODO Auto-generated constructor stub
	}

	// 重写add
	@Override
	protected void add(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		organizationComponents.add(organizationComponent);
	}

	// 重写remove
	@Override
	protected void remove(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		organizationComponents.remove(organizationComponent);
	}

	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return super.getName();
	}

	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		return super.getDes();
	}

	// print方法，就是输出University 包含的学院
	@Override
	protected void print() {
		// TODO Auto-generated method stub
		System.out.println("--------------" + getName() + "--------------");
		//遍历 organizationComponents 
		for (OrganizationComponent organizationComponent : organizationComponents) {
			organizationComponent.print();
		}
	}

}





public class Department extends OrganizationComponent {

	//没有集合
	
	public Department(String name, String des) {
		super(name, des);
		// TODO Auto-generated constructor stub
	}

	
	//add , remove 就不用写了，因为他是叶子节点
	
	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return super.getName();
	}
	
	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		return super.getDes();
	}
	
	@Override
	protected void print() {
		// TODO Auto-generated method stub
		System.out.println(getName());
	}

}





import java.util.ArrayList;
import java.util.List;

public class College extends OrganizationComponent {

	//List 中 存放的Department
	List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

	// 构造器
	public College(String name, String des) {
		super(name, des);
		// TODO Auto-generated constructor stub
	}

	// 重写add
	@Override
	protected void add(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		//  将来实际业务中，Colleage 的 add 和  University add 不一定完全一样
		organizationComponents.add(organizationComponent);
	}

	// 重写remove
	@Override
	protected void remove(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		organizationComponents.remove(organizationComponent);
	}

	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return super.getName();
	}

	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		return super.getDes();
	}

	// print方法，就是输出University 包含的学院
	@Override
	protected void print() {
		// TODO Auto-generated method stub
		System.out.println("--------------" + getName() + "--------------");
		//遍历 organizationComponents 
		for (OrganizationComponent organizationComponent : organizationComponents) {
			organizationComponent.print();
		}
	}


}





public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		//从大到小创建对象 学校
		OrganizationComponent university = new University("清华大学", " 中国顶级大学 ");
		
		//创建 学院
		OrganizationComponent computerCollege = new College("计算机学院", " 计算机学院 ");
		OrganizationComponent infoEngineercollege = new College("信息工程学院", " 信息工程学院 ");
		
		
		//创建各个学院下面的系(专业)
		computerCollege.add(new Department("软件工程", " 软件工程不错 "));
		computerCollege.add(new Department("网络工程", " 网络工程不错 "));
		computerCollege.add(new Department("计算机科学与技术", " 计算机科学与技术是老牌的专业 "));
		
		//
		infoEngineercollege.add(new Department("通信工程", " 通信工程不好学 "));
		infoEngineercollege.add(new Department("信息工程", " 信息工程好学 "));
		
		//将学院加入到 学校
		university.add(computerCollege);
		university.add(infoEngineercollege);
		
		//university.print();
		infoEngineercollege.print();
	}

}
```

#### 组合模式在JDK集合的源码分析

Java的集合类-HashMap就使用了组合模式

![HashMap使用组合模式](https://cdn.jsdelivr.net/gh/Turbo-King/images/HashMap使用组合模式.png "HashMap使用组合模式")



#### 组合模式的注意事项和细节

{{< admonition tip 组合模式 >}}

1. **简化客户端操作**。客户端只需要面对一致的对象而不用考虑整体部分或者节点叶子的问题
2. **具有较强的扩展性**。当我们要更改组合对象时，我们只需要调整内部的层次关系，客户端不用做出任何改变
3. **方便创建出复杂的层次结构**。客户端不用理会组合里面的组成细节，容易添加节点或者叶子从而创建出复杂的树形结构
4. **需要遍历组织结构**，或者处理的对象具有树形结构时，非常适合使用组合模式
5. **要求较高的抽象性**，**如果节点和叶子有很多差异性的话**，比如很多方法和属性都不一样，不适合使用组合模式

{{< /admonition >}}



<br>

### 外观模式

#### 传统方法

##### 影院管理项目

**组建一个家庭影院：**

DVD播放器、投影仪、自动屏幕、环绕立体声、爆米花机，要求完成使用家庭影院的功能，其过程如下：

- 直接用遥控器：统筹各系统开关
- 开爆米花机
- 放下投影幕布
- 开投影仪
- 开音响
- 开DVD，选DVD片源
- 去拿爆米花
- 调暗灯关
- 播放
- 观影结束，关闭各种设备



##### 传统方式解决影院管理

![传统方式解决影院管理](https://cdn.jsdelivr.net/gh/Turbo-King/images/传统方式解决影院管理.png "传统方式解决影院管理")

##### 传统方式解决影院管理问题分析

1. 在ClientTest的main方法中，创建各个子系统的对象，并直接去调用子系统（对象）相关方法，会造成调用过程混乱，没有清晰的过程
2. 不利于在ClientTest中，去维护对子系统的操作
3. 解决思路：定义一个高层接口，给子系统中的一组接口提供一个**一致的界面（比如在高层接口提供四个方法：ready，play，pause，end）**，用来访问子系统中的一群接口
4. 就是通过定义一个一致的接口（界面类），用以屏蔽内部子系统的细节，使得调用端只需跟这个接口发生调用，而无须关心这个子系统的内部细节->外观模式

#### 外观模式

##### 基本介绍

1. 外观模式（Facade），也叫“过程模式：外观模式为子系统中的一组接口提供一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更加容易使用”
2. 外观模式通过定义个一致的接口，用以屏蔽内部子系统的细节，使得调用端只需跟这个接口发生调用，而无需关心这个子系统的内部细节

##### 外观模式原理类图

![外观模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/外观模式原理类图.png "外观模式原理类图")

{{< admonition question 外观模式的角色及职责 >}}

1. 外观类（Facade）：为调用端提供统一的调用接口，外观类知道哪些子系统负责处理请求，从而将调用端的请求代理给适当子系统对象
2. 调用着（Client）：外观接口的调用者
3. 子系统的集合：指模块或者子系统，处理Facade对象指派的任务，它是功能的实际提供者

{{< /admonition >}}

##### 外观模式解决影院管理

1. 外观模式可以理解为转换一群接口，客户只要调用一个接口，而不用调用多个接口才能达到目的。比如：在PC上安装软件的时候经常有一键安装选项（省去选择安装目录、安装的组件等等），还有就是手机的重启功能（把关机和启动合为一个操作）
2. 外观模式就是解决多个复杂接口带来的使用困难，起到简化用户操作的作用
3. 示意图说明如下：

![外观模式解决影院管理示意图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E5%BD%B1%E9%99%A2%E7%AE%A1%E7%90%86%E7%A4%BA%E6%84%8F%E5%9B%BE.png "外观模式解决影院管理示意图")

##### 外观模式解决影院管理应用实例

![外观模式解决影院管理应用实例](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E5%BD%B1%E9%99%A2%E7%AE%A1%E7%90%86%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B.png "外观模式解决影院管理应用实例")

```java
public class HomeTheaterFacade {
	
	//定义各个子系统对象
	private TheaterLight theaterLight;
	private Popcorn popcorn;
	private Stereo stereo;
	private Projector projector;
	private Screen screen;
	private DVDPlayer dVDPlayer;
	
	
	//构造器
	public HomeTheaterFacade() {
		super();
		this.theaterLight = TheaterLight.getInstance();
		this.popcorn = Popcorn.getInstance();
		this.stereo = Stereo.getInstance();
		this.projector = Projector.getInstance();
		this.screen = Screen.getInstance();
		this.dVDPlayer = DVDPlayer.getInstanc();
	}

	//操作分成 4 步
	
	public void ready() {
		popcorn.on();
		popcorn.pop();
		screen.down();
		projector.on();
		stereo.on();
		dVDPlayer.on();
		theaterLight.dim();
	}
	
	public void play() {
		dVDPlayer.play();
	}
	
	public void pause() {
		dVDPlayer.pause();
	}
	
	public void end() {
		popcorn.off();
		theaterLight.bright();
		screen.up();
		projector.off();
		stereo.off();
		dVDPlayer.off();
	}

}






public class Stereo {

	private static Stereo instance = new Stereo();
	
	public static Stereo getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" Stereo on ");
	}
	
	public void off() {
		System.out.println(" Screen off ");
	}
	
	public void up() {
		System.out.println(" Screen up.. ");
	}
	
	//...
}





public class DVDPlayer {
	
	//使用单例模式, 使用饿汉式
	private static DVDPlayer instance = new DVDPlayer();
	
	public static DVDPlayer getInstanc() {
		return instance;
	}
	
	public void on() {
		System.out.println(" dvd on ");
	}
	public void off() {
		System.out.println(" dvd off ");
	}
	
	public void play() {
		System.out.println(" dvd is playing ");
	}
	
	//....
	public void pause() {
		System.out.println(" dvd pause ..");
	}
}





public class TheaterLight {

	private static TheaterLight instance = new TheaterLight();

	public static TheaterLight getInstance() {
		return instance;
	}

	public void on() {
		System.out.println(" TheaterLight on ");
	}

	public void off() {
		System.out.println(" TheaterLight off ");
	}

	public void dim() {
		System.out.println(" TheaterLight dim.. ");
	}

	public void bright() {
		System.out.println(" TheaterLight bright.. ");
	}
}




public class Projector {

	private static Projector instance = new Projector();
	
	public static Projector getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" Projector on ");
	}
	
	public void off() {
		System.out.println(" Projector ff ");
	}
	
	public void focus() {
		System.out.println(" Projector is Projector  ");
	}
	
	//...
}





public class Popcorn {
	
	private static Popcorn instance = new Popcorn();
	
	public static Popcorn getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" popcorn on ");
	}
	
	public void off() {
		System.out.println(" popcorn ff ");
	}
	
	public void pop() {
		System.out.println(" popcorn is poping  ");
	}
}






public class Screen {

	private static Screen instance = new Screen();
	
	public static Screen getInstance() {
		return instance;
	}
	
	public void up() {
		System.out.println(" Screen up ");
	}
	
	public void down() {
		System.out.println(" Screen down ");
	}
	
}
```

#### 外观模式在MyBatis框架应用的源码分析

Mybatis中的Configuration去创建MetaObject对象使用到外观模式

![mybatis中使用外观模式角色类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/mybatis%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F%E8%A7%92%E8%89%B2%E7%B1%BB%E5%9B%BE.png "mybatis中使用外观模式角色类图")

```java
public class Configuration {  
  protected ReflectorFactory reflectorFactory = new DefaultReflectorFactory();
  protected ObjectFactory objectFactory = new DefaultObjectFactory();
  protected ObjectWrapperFactory objectWrapperFactory = new DefaultObjectWrapperFactory();
  
  public MetaObject newMetaObject(Object object) {
    return MetaObject.forObject(object, objectFactory, objectWrapperFactory, reflectorFactory);
  }
}




public class MetaObject {
  private MetaObject(Object object, ObjectFactory objectFactory, ObjectWrapperFactory objectWrapperFactory, ReflectorFactory reflectorFactory) {
    this.originalObject = object;
    this.objectFactory = objectFactory;
    this.objectWrapperFactory = objectWrapperFactory;
    this.reflectorFactory = reflectorFactory;

    if (object instanceof ObjectWrapper) {
      this.objectWrapper = (ObjectWrapper) object;
    } else if (objectWrapperFactory.hasWrapperFor(object)) {
      this.objectWrapper = objectWrapperFactory.getWrapperFor(this, object);
    } else if (object instanceof Map) {
      this.objectWrapper = new MapWrapper(this, (Map) object);
    } else if (object instanceof Collection) {
      this.objectWrapper = new CollectionWrapper(this, (Collection) object);
    } else {
      this.objectWrapper = new BeanWrapper(this, object);
    }
  }
}
```

#### 外观模式的注意事项和细节

{{< admonition tip 外观模式 >}}

1. **外观模式**对外**屏蔽了子系统的的细节**，因此外观模式降低了客户端对子系统使用的复杂性
2. 外观模式对客户端与子系统的耦合关系，让子系统内部的模块更易维护和扩展
3. 通过合理的使用外观模式，可以帮我们更好的**划分访问的层次**
4. 当系统需要进行**分层设计**时，可以考虑使用Facade模式
5. 在维护一个遗留的大型系统时，可能这个系统已经变得非常难以维护和扩展，此时可以考虑为新系统开发一个Facade类，来提供遗留系统的比较清晰简单的接口，**让新系统与Facade类交互，提高复用性**
6. 不能过多的或者不合理的使用外观模式，使用外观模式好，还是直接调用模块好。**要以让系统有层次，利于维护为目的**

{{< /admonition >}}

<br>

### 享元模式

#### 传统方法

##### 展示网站项目需求

小型的外包项目，给客户A做一个产品展示网站，客户A的朋友感觉效果不错，也希望做这样的产品展示网站，但是要求都有些不同：

1. 客户要求以新闻的形式发布
2. 有客户人要求以博客的形式发布
3. 有客户希望以微信公众号的形式发布

##### 传统方案解决网站展现项目

1. 直觉复制粘贴一份，然后根据客户不同要求，进行定制修改
2. 给每个网站租用一个空间
3. 方案设计示意图如下：

![展示网站方案设计示意图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%B1%95%E7%A4%BA%E7%BD%91%E7%AB%99%E6%96%B9%E6%A1%88%E8%AE%BE%E8%AE%A1%E7%A4%BA%E6%84%8F%E5%9B%BE.png "展示网站方案设计示意图")

##### 传统方案解决网站展现项目问题分析

1. 需要的网站结构**相似度很高**，而且都**不是高访问量网站**，如果分成多个虚拟空间来处理，相当于一个相同的网站的实例对象有很多，造成服务器的资源浪费
2. 解决思路：**整合到一个网站中**，**共享**其相关的代码和数据，对于硬盘、内存、CPu、数据库空间等服务器资源都可以达到共享，减少服务器资源
3. 对于上面代码来说，由于是一份实例，维护和扩展都更加容易
4. 上面的解决思路就可以使用**享元模式**来解决

#### 享元模式

##### 基本介绍

1. 享元模式（Flyweight Pattern）也叫蝇量模式，运用共享技术有效地支持大量细粒度的对象
2. 常用于系统底层开发，解决系统的性能问题。像数据库连接池，里面都是创建好的连接对象，在这些连接对象中我们需要的则直接拿来用，避免重新创建，如果没有我们需要的，则创建一个
3. 享元模式能够解决重复对象的内存浪费问题，当系统中有大量相似对象，需要缓存池时。不需总是创建新对象，可以从缓存池里拿。这样可以降低系统内存，同时提高效率
4. 享元模式经典的应用常见就是池技术了，String常量池、数据库连接池、缓冲池等等都是享元模式的应用，享元吗模式是池技术的重要实现方式

![String常量池](https://cdn.jsdelivr.net/gh/Turbo-King/images/String%E5%B8%B8%E9%87%8F%E6%B1%A0.png "String常量池")

##### 享元模式的原理类图

![享元模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BA%AB%E5%85%83%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png "享元模式原理类图")

{{< admonition question 享元模式的角色及职责 >}}

1. **FlyWeight**是抽象的**享元角色**，它是产品的**抽象类**，同时定义出对象的**外部状态**和**内部状态**的接口或实现
2. **ConcreteFlyWeight**是**具体的享元角色**，是具体的产品类，实现抽象角色定义相关业务
3. **UnSharedConcreteFlyWeight**是不可共享的角色，一般不会出现在享元工厂
4. **FlyWeightFactory**享元**工厂类**，用于**创建一个池容器**（集合），同时提供从池中获取对象方法

{{< /admonition >}}

##### 内部状态和外部状态

比如围棋、五子棋、跳棋、它们都有大量的棋子对象，围棋和五子棋**只有黑白两色**，跳棋颜色多一点，所以棋子颜色就是棋子的**内部状态**；而各个棋子之间的差别就是位置的不同，当我们落子后，**落子颜色是定的，但位置是变化的**，所以棋子坐标就是棋子的**外部状态**

1. 享元模式提出两个要求：**细粒度**和**共享对象**。这里就涉及到内部状态和外部状态了，即将对象的信息分为两个部分，**内部状态**和**外部状态**
2. **内部状态**指对象**共享出来的信息**，存储在享元对象内部**且不会随环境的改变而变化**
3. **外部状态**指对象得以**依赖的一个标记**，是**随环境改变而变化的、不可共享的状态**
4. 举个例子：围棋理论上有361个空位可以放棋子，每个棋盘都有可能有两三百个棋子对象产生，因为内存空间有限，一台服务器很难支持更多的玩家玩围棋游戏，如果用享元模式来处理棋子，那么棋子对象就可以减少到只有两个实例，这样就很好的解决了对象的开销问题

##### 享元模式解决网站展现项目应用实例

![享元模式解决网站展现类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BA%AB%E5%85%83%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E7%BD%91%E7%AB%99%E5%B1%95%E7%8E%B0%E7%B1%BB%E5%9B%BE.png "享元模式解决网站展现类图")

```java
import java.util.HashMap;

// 网站工厂类，根据需要返回压一个网站
public class WebSiteFactory {

	
	//集合， 充当池的作用
	private HashMap<String, ConcreteWebSite> pool = new HashMap<>();
	
	//根据网站的类型，返回一个网站, 如果没有就创建一个网站，并放入到池中,并返回
	public WebSite getWebSiteCategory(String type) {
		if(!pool.containsKey(type)) {
			//就创建一个网站，并放入到池中
			pool.put(type, new ConcreteWebSite(type));
		}
		
		return (WebSite)pool.get(type);
	}
	
	//获取网站分类的总数 (池中有多少个网站类型)
	public int getWebSiteCount() {
		return pool.size();
	}
}






public abstract class WebSite {

	public abstract void use(User user);//抽象方法
}





//具体网站
public class ConcreteWebSite extends WebSite {

	//共享的部分，内部状态
	private String type = ""; //网站发布的形式(类型)

	
	//构造器
	public ConcreteWebSite(String type) {
		
		this.type = type;
	}


	@Override
	public void use(User user) {
		// TODO Auto-generated method stub
		System.out.println("网站的发布形式为:" + type + " 在使用中 .. 使用者是" + user.getName());
	}
	
	
}





public class User {
	
	private String name;

	
	public User(String name) {
		super();
		this.name = name;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
}



public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		// 创建一个工厂类
		WebSiteFactory factory = new WebSiteFactory();

		// 客户要一个以新闻形式发布的网站
		WebSite webSite1 = factory.getWebSiteCategory("新闻");

		
		webSite1.use(new User("tom"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite2 = factory.getWebSiteCategory("博客");

		webSite2.use(new User("jack"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite3 = factory.getWebSiteCategory("博客");

		webSite3.use(new User("smith"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite4 = factory.getWebSiteCategory("博客");

		webSite4.use(new User("king"));
		
		System.out.println("网站的分类共=" + factory.getWebSiteCount());
	}

}
```

#### 享元模式在JDK-Interge的应用源码分析

```java
public class FlyWeight {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//如果 Integer.valueOf(x) x 在  -128 --- 127 直接，就是使用享元模式返回,如果不在
		//范围类，则仍然 new 
		
		//小结:
		//1. 在valueOf 方法中，先判断值是否在 IntegerCache 中，如果不在，就创建新的Integer(new), 否则，就直接从 缓存池返回
		//2. valueOf 方法，就使用到享元模式
		//3. 如果使用valueOf 方法得到一个Integer 实例，范围在 -128 - 127 ，执行速度比 new 快
		
		
		Integer x = Integer.valueOf(127); // 得到 x实例，类型 Integer
		Integer y = new Integer(127); // 得到 y 实例，类型 Integer
		Integer z = Integer.valueOf(127);//..
		Integer w = new Integer(127);
		
		
		
		System.out.println(x.equals(y)); // 大小，true
		System.out.println(x == y ); //  false
		System.out.println(x == z ); // true
		System.out.println(w == x ); // false
		System.out.println(w == y ); // false
		
		
		Integer x1 = Integer.valueOf(200);
		Integer x2 = Integer.valueOf(200);
		System.out.println("x1==x2" + (x1 == x2)); // false

	}

}
```

#### 享元模式的注意事项和细节

{{< admonition tip 享元模式 >}}

1. 在享元模式这样理解，**“享”就表示共享**，**“元”表示对象**
2. 系统中有大量对象，这些对象消耗大量内存，并且**对象的状态大部分可以外部化**时，我们就可以考虑选用享元模式
3. 用唯一标识码判断，如果在内存中有，则返回这个**唯一标识码**所标识的对象，用**HashMap**/**HashTable**存储
4. 享元模式大大减少了对象的创建，降低了程序内存的占用，提高效率
5. 享元模式提高了系统的复杂度。需要分离出内部状态和外部状态，而**外部状态具有固化特性**，**不应该随着内部状态的改变而变化**，这就是我们使用享元模式需要注意的地方
6. 使用享元模式时，注意划分**内部状态**和**外部状态**，并且需要有一个**工厂类**加以控制
7. 享元模式经典的应用场景是需要**缓冲池**的场景，比如**String常量池**、**数据库连接池**

{{< /admonition >}}

<br>

### 代理模式

#### 基本介绍

1. 代理模式(Proxy)：为一个对象**提供一个替身**，以控制对这个对象的访问。即通过代理对象访问目标对象，这样做的**好处**是：可以在目标对象实现的基础上，增强额外的功能操作，即扩展目标对象的功能。
2. 被代理的对象可以是远程对象、**创建开销大的对象**或**需要安全控制的对象**。
3. 代理模式有不同的形式，主要有三种：静态代理、动态代理（JDK代理、接口代理）和Cglib代理（可以在内存动态的创建对象，而不需要实现接口，它是属于动态代理的范畴）
4. 代理模式示意图如下：

![代理模式示意图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F%E7%A4%BA%E6%84%8F%E5%9B%BE.png "代理模式示意图")



<br>

#### 静态代理

##### 基本介绍

静态代理在使用时，需要定义接口或者父类，被代理对象（即目标对象）与代理对象一起实现相同的接口或者是继承相同父类

##### 应用实例

具体要求

- 定义一个接口：ITeacherDao
- 目标对象TeacherDao实现接口ITeacherDAO
- 使用静态代理方式，就需要在代理对象TeacherDAOProxy中也实现ITeacherDAO
- 调用的时候通过调用代理对象的方法来调用目标对象
- **特别提醒**：代理对象与目标对象要实现相同的接口，然后通过调用相同的方法来调用目标对象的方法

![静态代理分类图解（类图）](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E9%9D%99%E6%80%81%E4%BB%A3%E7%90%86%E5%88%86%E7%B1%BB%E5%9B%BE%E8%A7%A3%EF%BC%88%E7%B1%BB%E5%9B%BE%EF%BC%89.png "静态代理分类图解（类图）")

```java
//代理对象,静态代理
public class TeacherDaoProxy implements ITeacherDao{
	
	private ITeacherDao target; // 目标对象，通过接口来聚合
	
	
	//构造器
	public TeacherDaoProxy(ITeacherDao target) {
		this.target = target;
	}

	@Override
	public void teach() {
		// TODO Auto-generated method stub
		System.out.println("开始代理  完成某些操作。。。。。 ");//方法
		target.teach();
		System.out.println("提交。。。。。");//方法
	}

}




//接口
public interface ITeacherDao {
	
	void teach(); // 授课的方法
}




public class TeacherDao implements ITeacherDao {

	@Override
	public void teach() {
		// TODO Auto-generated method stub
		System.out.println(" 老师授课中  。。。。。");
	}

}




public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建目标对象(被代理对象)
		TeacherDao teacherDao = new TeacherDao();
		
		//创建代理对象, 同时将被代理对象传递给代理对象
		TeacherDaoProxy teacherDaoProxy = new TeacherDaoProxy(teacherDao);
		
		//通过代理对象，调用到被代理对象的方法
		//即：执行的是代理对象的方法，代理对象再去调用目标对象的方法 
		teacherDaoProxy.teach();
	}

}
```

**静态代理优缺点**

1. **优点**：在不修改目标对象的功能前提下，能通过代理对象对目标功能扩展
2. **缺点**：因为代理对象需要与目标对象实现一样的接口，所以会有很多代理类
3. 一旦接口增加方法，目标对象与代理对象都要维护



<br>

#### 动态代理

##### 基本介绍

1. 代理对象，不需要实现接口，但是目标对象要实现接口，否则不能用动态代理
2. 代理对象的生成，是利用JDK的API，动态的在内存中构建代理对象
3. 动态代理也叫做：JDK代理、接口代理

##### JDK中生成代理对象的API

1. 代理类所在包：java.lang.reflect.Proxy
2. JDK实现代理只需要使用newProxyInstance方法，但是该方法需要接收三个参数，完成写法如下：

```java
static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler handler)
```

##### 应用实例

将前面的静态代理该进成动态代理模式（即：JDK代理模式）

![动态代理应用实例类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B%E7%B1%BB%E5%9B%BE.png "动态代理应用实例类图")



<br>

#### Cglib代理模式

##### 基本介绍

1. 静态代理和JDK代理模式都要求目标对象是实现一个接口，但是有时候目标对象只是一个单独的对象，并没有实现任何的接口，这个时候可使用目标对象子类来实现代理->这就是Cglib代理
2. Cglib代理也叫作子类代理，它是在内存中构建一个字类对象从而实现对目标对象功能扩展，有些书也将**Cglib代理归属到动态代理**
3. Cglib是一个强大的高性能的代码生成包，它可以在运行期扩展Java类与实现Java接口，它广泛的被许多AOP的框架使用，例如Spring AOP，实现方法拦截
4. 在AOP编程中如何选择代理模式
    - 目标对象需要实现接口，用JDK代理
    - 目标对象不需要实现接口，用Cglib代理

5. Cglib包的底层是通过使用字节码处理框架ASM来转换字节码并生成新的类

##### Cglib代理模式实现步骤

1. 需要引入cglib的jar包
2. 在内存中动态构建字类，注意代理的类不能为final，否则报错（java.lang.IllegalArgumentException）
3. 目标对象的方法如果为final/static，那么就不会被拦截，即不会执行目标对象额外的业务方法

##### Cglib代理模式应用实例

将前面案例用Cglib代理模式实现

![Cglib代理模式应用实例类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/Cglib%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B%E7%B1%BB%E5%9B%BE.png "Cglib代理模式应用实例类图")

```java
import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class ProxyFactory implements MethodInterceptor {

	//维护一个目标对象
	private Object target;
	
	//构造器，传入一个被代理的对象
	public ProxyFactory(Object target) {
		this.target = target;
	}

	//返回一个代理对象:  是 target 对象的代理对象
	public Object getProxyInstance() {
		//1. 创建一个工具类
		Enhancer enhancer = new Enhancer();
		//2. 设置父类
		enhancer.setSuperclass(target.getClass());
		//3. 设置回调函数
		enhancer.setCallback(this);
		//4. 创建子类对象，即代理对象
		return enhancer.create();
		
	}
	

	//重写  intercept 方法，会调用目标对象的方法
	@Override
	public Object intercept(Object arg0, Method method, Object[] args, MethodProxy arg3) throws Throwable {
		// TODO Auto-generated method stub
		System.out.println("Cglib代理模式 ~~ 开始");
		Object returnVal = method.invoke(target, args);
		System.out.println("Cglib代理模式 ~~ 提交");
		return returnVal;
	}

}




public class TeacherDao {

	public String teach() {
		System.out.println(" 老师授课中  ， 我是cglib代理，不需要实现接口 ");
		return "hello";
	}
}




public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建目标对象
		TeacherDao target = new TeacherDao();
		//获取到代理对象，并且将目标对象传递给代理对象
		TeacherDao proxyInstance = (TeacherDao)new ProxyFactory(target).getProxyInstance();

		//执行代理对象的方法，触发intecept 方法，从而实现 对目标对象的调用
		String res = proxyInstance.teach();
		System.out.println("res=" + res);
	}

}
```



<br>

#### 代理模式（Proxy）的变体

1. **防火墙代理**

    内网通过代理穿透防火墙，实现对公网的访问

2. **缓存代理**

    比如：当请求图片文件等资源时，先到缓存代理取，如果取到资源则OK，如果取不到资源，再到公网或者数据库中取，然后缓存

3. **远程代理**

    **远程对象的本地代表**，通过它可以**把远程对象当本地对象**来调用。远程代理通过网络和真正的远程对象沟通信息

4. 同步代理

    主要使用在多线程编程中，完成多线程间同步工作



<br>

### 模版方法模式

#### 豆浆制作问题

编写制作豆浆的程序，说明如下：

1. 制作豆浆的流程 选材->添加配料->浸泡->放到豆浆机打碎
2. 通过添加不同的配料，可以制作出不同口味的豆浆
3. 选材、浸泡和放到豆浆机打碎这几个步骤对于制作每种口味的豆浆都是一样的
4. 请使用**模版方法模式**完成（说明：因为模版方法模式，比较简单，很容易就想到这个方案，因此就直接使用，不再使用传统方案引出模版方法模式）

#### 模版方法模式基本介绍

1. 模版方法模式（Template Method Pattern），又叫模版模式（Template Pattern），在一个抽象类公开定义了执行它的方法的模版。它的字类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。
2. 简单说，模版方法模式定义了一个操作中的算法骨架，而将一些步骤延迟到子类中，使得子类可以不改变一个算法的结构，就可以重定义该算法的某些特定步骤
3. 这种类型的设计模式属于->行为型模式

#### 模版方法模式原理类图

![模版方法模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%A8%A1%E7%89%88%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png "模版方法模式原理类图")

{{< admonition question 模版方法模式的角色及职责 >}}

1. AbstractClass 抽象类，类中实现了模版方法（Template），定义了算法的骨架，具体子类需要去实现其它的抽象方法 operation2，3，4
2. ConcreteClass实现抽象方法operation2，3，4，以完成算法中特定子类的步骤

{{< /admonition >}}

#### 模版方法模式解决豆浆制作问题

![模版方法模式解决豆浆制作问题类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%A8%A1%E7%89%88%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E8%B1%86%E6%B5%86%E5%88%B6%E4%BD%9C%E9%97%AE%E9%A2%98%E7%B1%BB%E5%9B%BE.png "模版方法模式解决豆浆制作问题类图")

```java
//抽象类，表示豆浆
public abstract class SoyaMilk {

	//模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
	final void make() {
		
		select(); 
		addCondiments();
		soak();
		beat();
		
	}
	
	//选材料
	void select() {
		System.out.println("第一步：选择好的新鲜黄豆  ");
	}
	
	//添加不同的配料， 抽象方法, 子类具体实现
	abstract void addCondiments();
	
	//浸泡
	void soak() {
		System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
	}
	 
	void beat() {
		System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
	}
}




public class RedBeanSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的红豆 ");
	}

}




public class PeanutSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的花生 ");
	}

}




public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//制作红豆豆浆
		
		System.out.println("----制作红豆豆浆----");
		SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
		redBeanSoyaMilk.make();
		
		System.out.println("----制作花生豆浆----");
		SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
		peanutSoyaMilk.make();
	}

}

```

#### 模版方法模式的钩子方法

1. 在模版方法模式的父类中，我们定义一个方法，它默认不做任何事，子类可以视情况要不要覆盖它，该方法称为“钩子”
2. 还是用上面做豆浆的例子来讲解，比如，我们还希望制作纯豆浆，不添加任何的配料，请使用钩子方法对前面的模版方法进行改造

```java
//抽象类，表示豆浆
public abstract class SoyaMilk {

	//模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
	final void make() {
		
		select(); 
		if(customerWantCondiments()) {
			addCondiments();
		}
		soak();
		beat();
		
	}
	
	//选材料
	void select() {
		System.out.println("第一步：选择好的新鲜黄豆  ");
	}
	
	//添加不同的配料， 抽象方法, 子类具体实现
	abstract void addCondiments();
	
	//浸泡
	void soak() {
		System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
	}
	 
	void beat() {
		System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
	}
	
	//钩子方法，决定是否需要添加配料
	boolean customerWantCondiments() {
		return true;
	}
}





public class RedBeanSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的红豆 ");
	}

}





public class PureSoyaMilk extends SoyaMilk{

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		//空实现
	}
	
	@Override
	boolean customerWantCondiments() {
		// TODO Auto-generated method stub
		return false;
	}
 
}





public class PeanutSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的花生 ");
	}

}





public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//制作红豆豆浆
		
		System.out.println("----制作红豆豆浆----");
		SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
		redBeanSoyaMilk.make();
		
		System.out.println("----制作花生豆浆----");
		SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
		peanutSoyaMilk.make();
		
		System.out.println("----制作纯豆浆----");
		SoyaMilk pureSoyaMilk = new PureSoyaMilk();
		pureSoyaMilk.make();
	}

}
```

#### 模版方法模式在Spring框架应用的源码分析

**Spring IOC容器初始化时运用到的模版方法模式**

![Spring源码使用模版方法模式类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/Spring%E6%BA%90%E7%A0%81%E4%BD%BF%E7%94%A8%E6%A8%A1%E7%89%88%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F%E7%B1%BB%E5%9B%BE.png "Spring源码使用模版方法模式类图")

#### 模版方法模式注意事项和细节

{{< admonition tip 模版方法模式 >}}

1. **基本思想**：**算法只存在于一个地方，也就是在父类中，容易修改**。需要修改算法时，只要修改父类的模版方法或者已经实现的某些步骤，子类就会继承这些修改
2. **实现了最大化代码复用**。父类的模版方法和已经实现的某些步骤会被子类继承而直接使用
3. **即统一了算法，也提供了很大的灵活性**。父类的模版方法确保了算法的结构保持不变，同时由子类提供部分步骤的实现
4. 该模式的不足之处：每一个不同的实现都需要一个子类实现，导致类的个数增加，使得系统更加庞大
5. 一般模版方法都加上**final**关键字，防止子类重写模版方法
6. 模版方法模式使用场景：**当要完成在某个过程**，该过程要执行一系列步骤，**这一系列的步骤基本相同**，但其**个别步骤**在实现时可能不同，通常考虑用模版方法模式来处理

{{< /admonition >}}



<br>

### 命令模式

#### 智能生活项目

智能生活项目具体需求

1. 我们买了一套智能家电，有照明灯、风扇冰箱、洗衣机，我们只要在手机上安装app就可以控制对这些家电工作
2. 这些职能家电来自不同的厂家，我们不想针对每一种家电都安装一个App，分别控制，我们希望只要一个app就可以控制全部职能家电
3. 要实现一个app控制所有智能家电的需求，则每个智能家电厂家都要提供一个统一的接口给app调用，这时就可以考虑使用命令模式
4. 命令模式可将“动作的请求者”从“动作的执行者”对象中解耦出来
5. 在上面例子中，动作的请求者是手机app，动作的执行者是每个厂商的一个家电产品

#### 命令模式基本介绍

1. 命令模式（Command Pattern）：在软件设计中，我们经常需要向某些对象发送请求，但是并不知道请求的接收者是谁，也不知道被请求的操作是哪个，我们只需在程序运行时指定具体的请求接收者即可，此时，可以使用命令模式来进行设计
2. 命令模式使得请求发送者与请求接收者消除彼此之间的耦合，让对象之间的调用关系更加灵活，实现解耦
3. 在命令模式中，会将一个请求封装为一个对象，以便使用不同参数来表示不同的请求（即命名），同时命令模式也支持可撤销的操作
4. 通俗易懂的理解：将军发布命令，士兵去执行。其中有几个角色：将军（命令发布者）、士兵（命令的具体执行者）、命令（连接将军和士兵）。Invoker是调用者（将军），Receiver是被调用者（士兵），MyCommand是命令，实现了Command接口，持有接收对象

#### 命令模式的原理类图

![命令模式的原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F%E7%9A%84%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png "命令模式的原理类图")

{{< admonition question 命令模式的角色及职责 >}}

1. Invoker：是调用者角色
2. Command：是命令角色，需要执行的所有命令都在这里，可以是接口或抽象类
3. Receiver：是接收者角色，知道如何实施和执行一个请求相关的操作
4. ConcreteCommand：将一个接收者对象与一个动作绑定，调用接收者相应的操作，实现execute

{{< /admonition >}}

#### 命令模式解决智能生活项目

![命令模式解决智能生活项目类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E6%99%BA%E8%83%BD%E7%94%9F%E6%B4%BB%E9%A1%B9%E7%9B%AE%E7%B1%BB%E5%9B%BE.png "命令模式解决智能生活项目类图")

```java
//创建命令接口
public interface Command {

	//执行动作(操作)
	public void execute();
	//撤销动作(操作)
	public void undo();
}




/**
 * 没有任何命令，即空执行: 用于初始化每个按钮, 当调用空命令时，对象什么都不做
 * 其实，这样是一种设计模式, 可以省掉对空判断
 * @author Administrator
 *
 */
public class NoCommand implements Command {

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		
	}

}





public class LightOffCommand implements Command {

	// 聚合LightReceiver

	LightReceiver light;

	// 构造器
	public LightOffCommand(LightReceiver light) {
			super();
			this.light = light;
		}

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		light.off();
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		light.on();
	}
}





public class LightOnCommand implements Command {

	//聚合LightReceiver
	
	LightReceiver light;
	
	//构造器
	public LightOnCommand(LightReceiver light) {
		super();
		this.light = light;
	}
	
	@Override
	public void execute() {
		// TODO Auto-generated method stub
		//调用接收者的方法
		light.on();
	}

	

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		//调用接收者的方法
		light.off();
	}

}





public class LightReceiver {

	public void on() {
		System.out.println(" 电灯打开了.. ");
	}
	
	public void off() {
		System.out.println(" 电灯关闭了.. ");
	}
}





public class RemoteController {

	// 开 按钮的命令数组
	Command[] onCommands;
	Command[] offCommands;

	// 执行撤销的命令
	Command undoCommand;

	// 构造器，完成对按钮初始化

	public RemoteController() {

		onCommands = new Command[5];
		offCommands = new Command[5];

		for (int i = 0; i < 5; i++) {
			onCommands[i] = new NoCommand();
			offCommands[i] = new NoCommand();
		}
	}

	// 给我们的按钮设置你需要的命令
	public void setCommand(int no, Command onCommand, Command offCommand) {
		onCommands[no] = onCommand;
		offCommands[no] = offCommand;
	}

	// 按下开按钮
	public void onButtonWasPushed(int no) { // no 0
		// 找到你按下的开的按钮， 并调用对应方法
		onCommands[no].execute();
		// 记录这次的操作，用于撤销
		undoCommand = onCommands[no];

	}

	// 按下开按钮
	public void offButtonWasPushed(int no) { // no 0
		// 找到你按下的关的按钮， 并调用对应方法
		offCommands[no].execute();
		// 记录这次的操作，用于撤销
		undoCommand = offCommands[no];

	}
	
	// 按下撤销按钮
	public void undoButtonWasPushed() {
		undoCommand.undo();
	}

}





public class TVOffCommand implements Command {

	// 聚合TVReceiver

	TVReceiver tv;

	// 构造器
	public TVOffCommand(TVReceiver tv) {
		super();
		this.tv = tv;
	}

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.off();
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.on();
	}
}





public class TVOnCommand implements Command {

	// 聚合TVReceiver

	TVReceiver tv;

	// 构造器
	public TVOnCommand(TVReceiver tv) {
		super();
		this.tv = tv;
	}

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.on();
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.off();
	}
}





public class TVReceiver {
	
	public void on() {
		System.out.println(" 电视机打开了.. ");
	}
	
	public void off() {
		System.out.println(" 电视机关闭了.. ");
	}
}






public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		//使用命令设计模式，完成通过遥控器，对电灯的操作
		
		//创建电灯的对象(接受者)
		LightReceiver lightReceiver = new LightReceiver();
		
		//创建电灯相关的开关命令
		LightOnCommand lightOnCommand = new LightOnCommand(lightReceiver);
		LightOffCommand lightOffCommand = new LightOffCommand(lightReceiver);
		
		//需要一个遥控器
		RemoteController remoteController = new RemoteController();
		
		//给我们的遥控器设置命令, 比如 no = 0 是电灯的开和关的操作
		remoteController.setCommand(0, lightOnCommand, lightOffCommand);
		
		System.out.println("--------按下灯的开按钮-----------");
		remoteController.onButtonWasPushed(0);
		System.out.println("--------按下灯的关按钮-----------");
		remoteController.offButtonWasPushed(0);
		System.out.println("--------按下撤销按钮-----------");
		remoteController.undoButtonWasPushed();
		
		
		System.out.println("=========使用遥控器操作电视机==========");
		
		TVReceiver tvReceiver = new TVReceiver();
		
		TVOffCommand tvOffCommand = new TVOffCommand(tvReceiver);
		TVOnCommand tvOnCommand = new TVOnCommand(tvReceiver);
		
		//给我们的遥控器设置命令, 比如 no = 1 是电视机的开和关的操作
		remoteController.setCommand(1, tvOnCommand, tvOffCommand);
		
		System.out.println("--------按下电视机的开按钮-----------");
		remoteController.onButtonWasPushed(1);
		System.out.println("--------按下电视机的关按钮-----------");
		remoteController.offButtonWasPushed(1);
		System.out.println("--------按下撤销按钮-----------");
		remoteController.undoButtonWasPushed();

	}

}
```

#### 命令模式在Spring框架JdbcTemplate应用的源码分析

**Spring框架的JdbcTemplate就使用到了命令模式**

```java
public class JdbcTemplate extends JdbcAccessor implements JdbcOperations {

public void query(String sql, RowCallbackHandler rch) throws DataAccessException {
        this.query((String)sql, (ResultSetExtractor)(new RowCallbackHandlerResultSetExtractor(rch)));
    }
  
  
  
  @Nullable
    public <T> T query(final String sql, final ResultSetExtractor<T> rse) throws DataAccessException {
        Assert.notNull(sql, "SQL must not be null");
        Assert.notNull(rse, "ResultSetExtractor must not be null");
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Executing SQL query [" + sql + "]");
        }

        class QueryStatementCallback implements StatementCallback<T>, SqlProvider {
            QueryStatementCallback() {
            }

            @Nullable
            public T doInStatement(Statement stmt) throws SQLException {
                ResultSet rs = null;

                Object var3;
                try {
                    rs = stmt.executeQuery(sql);
                    var3 = rse.extractData(rs);
                } finally {
                    JdbcUtils.closeResultSet(rs);
                }

                return var3;
            }

            public String getSql() {
                return sql;
            }
        }

        return this.execute(new QueryStatementCallback(), true);
    }
  
  

@Nullable
    public <T> T execute(ConnectionCallback<T> action) throws DataAccessException {
    
    ·······
    
    }
}





@FunctionalInterface
public interface StatementCallback<T> {
    @Nullable
    T doInStatement(Statement stmt) throws SQLException, DataAccessException;
}
```

{{< admonition question 模式角色分析说明 >}}

- StatementCallback接口，类似命令接口（Command）
- class QueryStatementCallback implements StatementCallback`<T>`,sqlProvider,匿名内部类，实现了命令接口，同时也充当命令接收者
- 命令调用者是JdbcTemplate，其中execute（StatementCallback`<T>`action）方法中，调用action.doInStatement方法。不同的实现StatementCallback接口的对象，对应不同的doInStatement实现逻辑
- 另外实现StatementCallback命令接口的子类还有QueryStatementCallback

![StatementCallback 命令接口的子类](https://cdn.jsdelivr.net/gh/Turbo-King/images/StatementCallback%20%E5%91%BD%E4%BB%A4%E6%8E%A5%E5%8F%A3%E7%9A%84%E5%AD%90%E7%B1%BB.png "StatementCallback 命令接口的子类")

{{< /admonition >}}

#### 命令模式的注意事项和细节

{{< admonition tip 命令模式 >}}

1. 将发起请求的对象与执行请求的对象解耦。发起请求的对象是调用者，调用者只要调用命令对象的execute()方法就可以让接收者工作，而不必知道具体的接收者对象是谁、是如何实现的，命令对象会负责让接收者执行请求的动作，也就是说：“请求发起者”和“请求执行者”之间的解耦是通过命令对象实现的，命令对象起到了纽带桥梁的作用
2. 容易设计一个命令队列。只要把命令对象放到队列，就可以多线程的执行命令
3. 容易实现对请求的撤销和重做
4. 命令模式不足：可能导致某些系统有过多的具体命令类，增加了系统的复杂度，这点在使用的时候需要注意
5. 空命令也是一种设计模式，它为我们省去了判空的操作。在上面的实例中，如果没有使用空命令，我们每按下一个按键都需要判空，这给我们编码带来了一定的麻烦
6. 命令模式经典的应用场景：界面的一个按钮都是一条命令、模拟CMD（DOS命令）订单的撤销/恢复、触发-反馈机制

{{< /admonition >}}





<br>

### 访问者模式

#### 评测系统

**评测系统需求**

将观众分为男人和女人，对歌手进行评测，当看完某个歌手表演后，得到他们对该歌手不同的评价（评价有不同的种类，比如成功、失败等）

#### 传统方法

##### 传统方式的问题分析

![评测系统传统方式解决](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%AF%84%E6%B5%8B%E7%B3%BB%E7%BB%9F%E4%BC%A0%E7%BB%9F%E6%96%B9%E5%BC%8F%E8%A7%A3%E5%86%B3.png "评测系统传统方式解决")

1. 如果系统比较小，还是OK的，但是考虑系统增加越来越多新的功能时，对代码改动比较大，违反了OCP原则，不利于维护
2. 扩展性不好，比如增加了新的人员类型，或者管理方法，都不好做
3. 引出我们会使用新的设计模式->访问者模式



#### 访问者模式

##### 基本介绍

1. **访问者模式（Visitor Patter）**，封装一些作用于某种数据结构的各元素的操作，它可以在不改变数据结构的前提下定义作用于这些元素的新的操作
2. 主要将数据结构与数据操作分离，解决**数据结构**和**操作耦合性**问题
3. **访问者模式的基本工作原理是**：在被访问的类里面加一个对外提供接待访问者的接口
4. **访问者模式应用场景是**：需要对一个对象结构中的对象进行很多不同操作（这些操作彼此没有关联），同时需要避免让这些操作“污染”这些对象的类，可以选用访问者模式解决

##### 访问者模式的原理类图

![访问者模式的原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8F%E7%9A%84%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png "访问者模式的原理类图")

{{< admonition question 访问者模式的角色及职责 >}}

1. Visitor：是抽象访问者，为该对象结构中的ConcreteElement的每一个类声明一个visit操作
2. ConcreteVisitor：是一个具体的访问值，实现每一个有Visitor声明的操作，是每个操作实现的部分
3. ObjectStructure：能枚举它的元素，可以提供一个高层的接口，用来允许访问者访问元素
4. Element：定义一个accept方法，接收一个访问者对象
5. ConcreteElement为具体元素，实现了accept方法

{{< /admonition >}}

##### 访问者模式应用实例

![访问者模式应用实例](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8F%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B-.png "访问者模式应用实例")

```java
public abstract class Action {
	
	//得到男性 的测评
	public abstract void getManResult(Man man);
	
	//得到女的 测评
	public abstract void getWomanResult(Woman woman);
}





public class Fail extends Action {

	@Override
	public void getManResult(Man man) {
		// TODO Auto-generated method stub
		System.out.println(" 男人给的评价该歌手失败 !");
	}

	@Override
	public void getWomanResult(Woman woman) {
		// TODO Auto-generated method stub
		System.out.println(" 女人给的评价该歌手失败 !");
	}

}





public abstract class Person {
	
	//提供一个方法，让访问者可以访问
	public abstract void accept(Action action);
}






public class Man extends Person {

	@Override
	public void accept(Action action) {
		// TODO Auto-generated method stub
		action.getManResult(this);
	}

}






//说明
//1. 这里我们使用到了双分派, 即首先在客户端程序中，将具体状态作为参数传递Woman中(第一次分派)
//2. 然后Woman 类调用作为参数的 "具体方法" 中方法getWomanResult, 同时将自己(this)作为参数
//   传入，完成第二次的分派
public class Woman extends Person{

	@Override
	public void accept(Action action) {
		// TODO Auto-generated method stub
		action.getWomanResult(this);
	}

}






public class Success extends Action {

	@Override
	public void getManResult(Man man) {
		// TODO Auto-generated method stub
		System.out.println(" 男人给的评价该歌手很成功 !");
	}

	@Override
	public void getWomanResult(Woman woman) {
		// TODO Auto-generated method stub
		System.out.println(" 女人给的评价该歌手很成功 !");
	}

}





import java.util.LinkedList;
import java.util.List;

//数据结构，管理很多人（Man , Woman）
public class ObjectStructure {

	//维护了一个集合
	private List<Person> persons = new LinkedList<>();
	
	//增加到list
	public void attach(Person p) {
		persons.add(p);
	}
	//移除
	public void detach(Person p) {
		persons.remove(p);
	}
	
	//显示测评情况
	public void display(Action action) {
		for(Person p: persons) {
			p.accept(action);
		}
	}
}








public class Wait extends Action {

	@Override
	public void getManResult(Man man) {
		// TODO Auto-generated method stub
		System.out.println(" 男人给的评价是该歌手待定 ..");
	}

	@Override
	public void getWomanResult(Woman woman) {
		// TODO Auto-generated method stub
		System.out.println(" 女人给的评价是该歌手待定 ..");
	}

}






public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建ObjectStructure
		ObjectStructure objectStructure = new ObjectStructure();
		
		objectStructure.attach(new Man());
		objectStructure.attach(new Woman());
		
		
		//成功
		Success success = new Success();
		objectStructure.display(success);
		
		System.out.println("===============");
		Fail fail = new Fail();
		objectStructure.display(fail);
		
		System.out.println("=======给的是待定的测评========");
		
		Wait wait = new Wait();
		objectStructure.display(wait);
	}

}
```





**应用实例小结**

- 上面提到了双分派，所谓双分派是指不管类怎么变化，我们都能找到期望的方法运行。
- 双分派意味着得到执行的操作取决于请求的种类和两个接收者的类型
- 对于以上实例为例，假设我们要添加一个Wait的状态类，考察Man类和Woman类的反应，由于使用了双分派，**只需增加一个Action子类即可在客户端调用即可，不需要改动任何其他类的代码**。

##### 访问者模式的注意事项和细节

{{< admonition tip 访问者模式 >}}

- 优点
    1. 访问者模式符合单一职责原则，让程序具有优秀的扩展性、灵活性非常高
    2. 访问者模式可以对功能进行统一，可以做报表、UI、拦截器与过滤器，适用于数据结构相对稳定的系统

- 缺点
    1. 具体元素对访问者公布细节，也就是说访问者关注了其他类的内部细节，这是迪米特法则所不建议的，这样就造成了具体元素变更比较困难
    2. 违背了依赖倒转原则。访问者依赖的是具体元素，而不是抽象元素
    3. 因此，如果一个系统有比较稳定的数据结构，又有经常变化的功能需求，那么访问者模式就是比较合适的

{{< /admonition >}}



<br>

### 迭代器模式

#### 学校院系展示系统

编写程序展示一个学校院系结构：要求在一个页面中展示出学校的院系组成，一个学校有多个学院，一个学院有多个系。功能展示如下所示。

![学校院系构成](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%AD%A6%E6%A0%A1%E9%99%A2%E7%B3%BB%E6%9E%84%E6%88%90.png "学校院系构成")

#### 传统的设计方案

![学校院系构成传统设计方案](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%AD%A6%E6%A0%A1%E9%99%A2%E7%B3%BB%E6%9E%84%E6%88%90%E4%BC%A0%E7%BB%9F%E8%AE%BE%E8%AE%A1%E6%96%B9%E6%A1%88.png "学校院系构成传统设计方案")

##### 传统方式的问题分析

1. 将学院看做是学校的子类，系是学院的子类，这样实际上是站在**组织大小**来进行分层次的
2. 实际上我们的需求是：在一个页面中展示出学校的院系组成，一个学校有多个学院，一个学院有多个系，因此这种方案，不能很好实现**遍历**的操作
3. 解决方案 -> **迭代器模式**



<br>

#### 迭代器模式

##### 基本介绍

1. **迭代器模式**（Iterator Pattern）是常用的设计模式，属于**行为型模式**
2. 如果我们的集合元素使用不同的方式实现，有数组，还有Java的集合类，或者还有其他方式，当客户端要**遍历这些集合元素的时候就要使用多种遍历方式**，而且还会**暴露元素的内部结构**，可以考虑使用迭代器模式解决
3. **迭代器模式**，提供一种遍历集合元素的**统一接口**，用一致的方式遍历集合元素，不需要知道集合对象的底层表示，即：**不暴露其内部的结构**

##### 迭代器模式原理类图

![迭代器模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png)

{{< admonition question 迭代器模式的角色及职责 >}}

1. **Iterator**：**迭代器接口**，是系统提供，含义 hasNext，next，remove
2. **ConcreteIterator**：具体的迭代器类，管理迭代
3. **Aggregate**：一个统一的**聚合接口**，将**客户端和具体聚合解耦**
4. **ConcreteAggreage**：具体的聚合持有对象集合，并提供一个方法，返回一个迭代器，该迭代器可以正确遍历集合
5. **Client**：客户端，**通过 Iterator 和 Aggregate 依赖子类**

{{< /admonition >}}



##### 迭代器模式应用实例

![迭代器模式应用实例](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B.png "迭代器模式应用实例")

```java
import java.util.Iterator;

public interface College {
	
	public String getName();
	
	//增加系的方法
	public void addDepartment(String name, String desc);
	
	//返回一个迭代器,遍历
	public Iterator  createIterator();
}





public class Department {

	private String name;
	private String desc;
	public Department(String name, String desc) {
		super();
		this.name = name;
		this.desc = desc;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getDesc() {
		return desc;
	}
	public void setDesc(String desc) {
		this.desc = desc;
	}
	
}






import java.util.Iterator;
import java.util.List;

public class OutPutImpl {
	
	//学院集合
	List<College> collegeList;

	public OutPutImpl(List<College> collegeList) {
		
		this.collegeList = collegeList;
	}
	//遍历所有学院,然后调用printDepartment 输出各个学院的系
	public void printCollege() {
		
		//从collegeList 取出所有学院, Java 中的 List 已经实现Iterator
		Iterator<College> iterator = collegeList.iterator();
		
		while(iterator.hasNext()) {
			//取出一个学院
			College college = iterator.next();
			System.out.println("=== "+college.getName() +"=====" );
			printDepartment(college.createIterator()); //得到对应迭代器
		}
	}
	
	
	//输出 学院输出 系
	
	public void printDepartment(Iterator iterator) {
		while(iterator.hasNext()) {
			Department d = (Department)iterator.next();
			System.out.println(d.getName());
		}
	}
	
}






import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class InfoCollege implements College {

	List<Department> departmentList;
	
	
	public InfoCollege() {
		departmentList = new ArrayList<Department>();
		addDepartment("信息安全专业", " 信息安全专业 ");
		addDepartment("网络安全专业", " 网络安全专业 ");
		addDepartment("服务器安全专业", " 服务器安全专业 ");
	}
	
	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return "信息工程学院";
	}

	@Override
	public void addDepartment(String name, String desc) {
		// TODO Auto-generated method stub
		Department department = new Department(name, desc);
		departmentList.add(department);
	}

	@Override
	public Iterator createIterator() {
		// TODO Auto-generated method stub
		return new InfoColleageIterator(departmentList);
	}

}







import java.util.Iterator;
import java.util.List;

public class InfoColleageIterator implements Iterator {

	
	List<Department> departmentList; // 信息工程学院是以List方式存放系
	int index = -1;//索引
	

	public InfoColleageIterator(List<Department> departmentList) {
		this.departmentList = departmentList;
	}

	//判断list中还有没有下一个元素
	@Override
	public boolean hasNext() {
		// TODO Auto-generated method stub
		if(index >= departmentList.size() - 1) {
			return false;
		} else {
			index += 1;
			return true;
		}
	}

	@Override
	public Object next() {
		// TODO Auto-generated method stub
		return departmentList.get(index);
	}
	
	//空实现remove
	public void remove() {
		
	}

}






import java.util.Iterator;

public class ComputerCollege implements College {

	Department[] departments;
	int numOfDepartment = 0 ;// 保存当前数组的对象个数
	
	
	public ComputerCollege() {
		departments = new Department[5];
		addDepartment("Java专业", " Java专业 ");
		addDepartment("PHP专业", " PHP专业 ");
		addDepartment("大数据专业", " 大数据专业 ");
		
	}
	
	
	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return "计算机学院";
	}

	@Override
	public void addDepartment(String name, String desc) {
		// TODO Auto-generated method stub
		Department department = new Department(name, desc);
		departments[numOfDepartment] = department;
		numOfDepartment += 1;
	}

	@Override
	public Iterator createIterator() {
		// TODO Auto-generated method stub
		return new ComputerCollegeIterator(departments);
	}

}





import java.util.Iterator;


public class ComputerCollegeIterator implements Iterator {

	//这里我们需要Department 是以怎样的方式存放=>数组
	Department[] departments;
	int position = 0; //遍历的位置
	
	
	
	
	public ComputerCollegeIterator(Department[] departments) {
		this.departments = departments;
	}

	//判断是否还有下一个元素
	@Override
	public boolean hasNext() {
		// TODO Auto-generated method stub
		if(position >= departments.length || departments[position] == null) {
			return false;
		}else {
		
			return true;
		}
	}

	@Override
	public Object next() {
		// TODO Auto-generated method stub
		Department department = departments[position];
		position += 1;
		return department;
	}
	
	//删除的方法，默认空实现
	public void remove() {
		
	}

}






import java.util.ArrayList;
import java.util.List;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建学院
		List<College> collegeList = new ArrayList<College>();
		
		ComputerCollege computerCollege = new ComputerCollege();
		InfoCollege infoCollege = new InfoCollege();
		
		collegeList.add(computerCollege);
		//collegeList.add(infoCollege);
		
		OutPutImpl outPutImpl = new OutPutImpl(collegeList);
		outPutImpl.printCollege();
	}

}
```



<br>

#### 迭代器模式在JDK-ArrayList集合应用的源码分析

JDK中的ArrayList集合中就使用了迭代器模式

```java
public interface Iterator<E> {
    /**
     * Returns {@code true} if the iteration has more elements.
     * (In other words, returns {@code true} if {@link #next} would
     * return an element rather than throwing an exception.)
     *
     * @return {@code true} if the iteration has more elements
     */
    boolean hasNext();

    /**
     * Returns the next element in the iteration.
     *
     * @return the next element in the iteration
     * @throws NoSuchElementException if the iteration has no more elements
     */
    E next();

    /**
     * Removes from the underlying collection the last element returned
     * by this iterator (optional operation).  This method can be called
     * only once per call to {@link #next}.  The behavior of an iterator
     * is unspecified if the underlying collection is modified while the
     * iteration is in progress in any way other than by calling this
     * method.
     *
     * @implSpec
     * The default implementation throws an instance of
     * {@link UnsupportedOperationException} and performs no other action.
     *
     * @throws UnsupportedOperationException if the {@code remove}
     *         operation is not supported by this iterator
     *
     * @throws IllegalStateException if the {@code next} method has not
     *         yet been called, or the {@code remove} method has already
     *         been called after the last call to the {@code next}
     *         method
     */
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }
}






public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
  private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

        @Override
        @SuppressWarnings("unchecked")
        public void forEachRemaining(Consumer<? super E> consumer) {
            Objects.requireNonNull(consumer);
            final int size = ArrayList.this.size;
            int i = cursor;
            if (i >= size) {
                return;
            }
            final Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length) {
                throw new ConcurrentModificationException();
            }
            while (i != size && modCount == expectedModCount) {
                consumer.accept((E) elementData[i++]);
            }
            // update once at end of iteration to reduce heap write traffic
            cursor = i;
            lastRet = i - 1;
            checkForComodification();
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
}






public interface List<E> extends Collection<E> {
    /**
     * Returns an iterator over the elements in this list in proper sequence.
     *
     * @return an iterator over the elements in this list in proper sequence
     */
    Iterator<E> iterator();
}
```

{{< admonition question ArrayList 中角色及职责 >}}

- 内部类Itr充当具体实现迭代器 Iterator 的类，作为 ArrayList 内部类
- List 就是充当了聚合接口，含有一个 Iterator() 方法，返回一个迭代器对象
- ArrayList是实现聚合接口List的子类，实现了 Iterator()
- Iterator 接口系统提供
- 迭代器模式解决了不同集合（ArrayList，LinkedList）统一遍历问题

{{< /admonition >}}



<br>

#### 迭代器模式的注意事项和细节

{{< admonition tip 迭代器模式 >}}

- 优点
    1. 提供一个统一的方法遍历对象，客户不用再考虑聚合的类型，使用一种方法就可以遍历对象了
    2. 隐藏了聚合的内部结构，客户端要遍历聚合的时候只能取到迭代器，而不会知道聚合的具体组成
    3. 提供了一种设计思想，就是一个类应该只有一个引起变化的原因（叫做单一责任原则）。在聚合类中，我们把迭代器分开，就是要把管理对象集合和遍历对象集合的责任分开，这样一来集合改变的话，只影响到聚合对象。而如果遍历方式改变的话，只影响到了迭代器
    4. 当要展示一组相似对象，或者遍历一组相同对象时使用，适合使用迭代器模式
- 缺点
    1. 每个聚合对象都要一个迭代器，会生成多个迭代器不好管理类

{{< /admonition >}}





<br>

### 观察者模式

#### 天气预报项目

**具体需求如下：**

1. 气象站可以将每天测量到的温度、湿度、气压等等以公告的形式发布出去（比如发布到自己的网站或第三方）
2. 需要设计开放型API，便于其他第三方也能接入气象站获取数据
3. 提供温度、气压和湿度的接口
4. 测量数据更新时，要能实时的通知给第三方



#### 传统方案

通过对气象站项目的分析，我们可以初步设计出一个WeatherData类

![weather](https://cdn.jsdelivr.net/gh/Turbo-King/images/weather.png "weather")

**说明：**

1. 通过getXxx方法，可以让第三方接入，并得到相关信息
2. 当数据有更新时，气象站通过调用 dataChange() 去个更新数据，当第三方再次获取时，就能得到最新数据，当然也可以推送



**解决示意图：**

![解决示意图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%A7%A3%E5%86%B3%E7%A4%BA%E6%84%8F%E5%9B%BE.png "解决示意图解决示意图")

CurrentConditions(当前的天气情况)

可以理解成是我们气象局的网站 // 推送

```java
/**
 * 显示当前天气情况（可以理解成是气象站自己的网站）
 * @author Administrator
 *
 */
public class CurrentConditions {
	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	//更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	//显示
	public void display() {
		System.out.println("***Today mTemperature: " + temperature + "***");
		System.out.println("***Today mPressure: " + pressure + "***");
		System.out.println("***Today mHumidity: " + humidity + "***");
	}
}





/**
 * 类是核心
 * 1. 包含最新的天气情况信息 
 * 2. 含有 CurrentConditions 对象
 * 3. 当数据有更新时，就主动的调用   CurrentConditions对象update方法(含 display), 这样他们（接入方）就看到最新的信息
 * @author Administrator
 *
 */
public class WeatherData {
	private float temperatrue;
	private float pressure;
	private float humidity;
	private CurrentConditions currentConditions;
	//加入新的第三方

	public WeatherData(CurrentConditions currentConditions) {
		this.currentConditions = currentConditions;
	}

	public float getTemperature() {
		return temperatrue;
	}

	public float getPressure() {
		return pressure;
	}

	public float getHumidity() {
		return humidity;
	}

	public void dataChange() {
		//调用 接入方的 update
		currentConditions.update(getTemperature(), getPressure(), getHumidity());
	}

	//当数据有更新时，就调用 setData
	public void setData(float temperature, float pressure, float humidity) {
		this.temperatrue = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		//调用dataChange， 将最新的信息 推送给 接入方 currentConditions
		dataChange();
	}
}






public class Client {
	public static void main(String[] args) {
		//创建接入方 currentConditions
		CurrentConditions currentConditions = new CurrentConditions();
		//创建 WeatherData 并将 接入方 currentConditions 传递到 WeatherData中
		WeatherData weatherData = new WeatherData(currentConditions);
		
		//更新天气情况
		weatherData.setData(30, 150, 40);
		
		//天气情况变化
		System.out.println("============天气情况变化=============");
		weatherData.setData(40, 160, 20);
		

	}
}
```

##### 传统方案问题分析

1. 其他第三方接入气象站获取数据的问题
2. 无法在运行时动态的添加第三方（新浪网站）
3. 违反 OCP 原则 -> 观察者模式

```java
// 在WeatherData中，当增加一个第三方，都需要创建一个对应的第三方的公告板对象，并加入到dataChange，不利于维护也不是动态加入
public void dataChange(){
  currentConditions.update(getTemperature,getPressure,getHumidity());
}
```



<br>

#### 观察者模式

##### 基本介绍

**观察者模式（Observer Pattern）**：指多个对象间存在**一对多的依赖关系**，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作**发布-订阅模式**、**模型-视图模式**，它是对象**行为型模式**。

##### 观察者模式原理

![观察者模式原理-Subject](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86.png "观察者模式原理-Subject")



![观察者模式原理-Observer](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86-Observer.png)

{{< admonition question 观察者模式的角色及职责 >}}

- **观察者模式类似用户订购牛奶业务**
    1. 奶站：**Subject**
    2. 用户：**Observer**
- **Subject**：登记注册、移除和通知
    1. **registerObserver** 注册
    2. **removeObserver** 移除
    3. **notifyObserver** 通知所有的注册用户，根据不同的需求，可以是更新数据，让用户来取，也可能是实施推送，根据具体需求定
- **Observer**：接收输入
- **观察者模式**：对象之间多对一依赖的一种设计方案，被依赖的对象为 Subject，依赖的对象为 Observer，Subject 通知 Observer 变化，比如这里的奶站是 Subject，是“1”的一方，用户是 Observer，是“多”的一方

{{< /admonition >}}

##### 观察者模式应用实例

**观察者模式解决天气预报需求**

![观察者模式解决天气预报需求类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E5%A4%A9%E6%B0%94%E9%A2%84%E6%8A%A5%E9%9C%80%E6%B1%82%E7%B1%BB%E5%9B%BE.png "观察者模式解决天气预报需求类图")

```java
//观察者接口，有观察者来实现
public interface Observer {

	public void update(float temperature, float pressure, float humidity);
}






//接口, 让WeatherData 来实现 
public interface Subject {
	
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}







import java.util.ArrayList;

/**
 * 类是核心
 * 1. 包含最新的天气情况信息 
 * 2. 含有 观察者集合，使用ArrayList管理
 * 3. 当数据有更新时，就主动的调用   ArrayList, 通知所有的（接入方）就看到最新的信息
 * @author Administrator
 *
 */
public class WeatherData implements Subject {
	private float temperatrue;
	private float pressure;
	private float humidity;
	//观察者集合
	private ArrayList<Observer> observers;
	
	//加入新的第三方

	public WeatherData() {
		observers = new ArrayList<Observer>();
	}

	public float getTemperature() {
		return temperatrue;
	}

	public float getPressure() {
		return pressure;
	}

	public float getHumidity() {
		return humidity;
	}

	public void dataChange() {
		//调用 接入方的 update
		
		notifyObservers();
	}

	//当数据有更新时，就调用 setData
	public void setData(float temperature, float pressure, float humidity) {
		this.temperatrue = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		//调用dataChange， 将最新的信息 推送给 接入方 currentConditions
		dataChange();
	}

	//注册一个观察者
	@Override
	public void registerObserver(Observer o) {
		// TODO Auto-generated method stub
		observers.add(o);
	}

	//移除一个观察者
	@Override
	public void removeObserver(Observer o) {
		// TODO Auto-generated method stub
		if(observers.contains(o)) {
			observers.remove(o);
		}
	}

	//遍历所有的观察者，并通知
	@Override
	public void notifyObservers() {
		// TODO Auto-generated method stub
		for(int i = 0; i < observers.size(); i++) {
			observers.get(i).update(this.temperatrue, this.pressure, this.humidity);
		}
	}
}






public class CurrentConditions implements Observer {

	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	// 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	// 显示
	public void display() {
		System.out.println("***Today mTemperature: " + temperature + "***");
		System.out.println("***Today mPressure: " + pressure + "***");
		System.out.println("***Today mHumidity: " + humidity + "***");
	}
}







public class BaiduSite implements Observer {

	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	// 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	// 显示
	public void display() {
		System.out.println("===百度网站====");
		System.out.println("***百度网站 气温 : " + temperature + "***");
		System.out.println("***百度网站 气压: " + pressure + "***");
		System.out.println("***百度网站 湿度: " + humidity + "***");
	}

}






public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建一个WeatherData
		WeatherData weatherData = new WeatherData();
		
		//创建观察者
		CurrentConditions currentConditions = new CurrentConditions();
		BaiduSite baiduSite = new BaiduSite();
		
		//注册到weatherData
		weatherData.registerObserver(currentConditions);
		weatherData.registerObserver(baiduSite);
		
		//测试
		System.out.println("通知各个注册的观察者, 看看信息");
		weatherData.setData(10f, 100f, 30.3f);
		
		
		weatherData.removeObserver(currentConditions);
		//测试
		System.out.println();
		System.out.println("通知各个注册的观察者, 看看信息");
		weatherData.setData(10f, 100f, 30.3f);
	}

}
```

**观察者模式的好处**

1. 观察者模式设计后，会以集合的方式来管理用户（Observer），包括注册，移除和通知
2. 我们增加观察者（这里可以理解成一个新的公告板），就不需要去修改和心类 WeatherData 不会修改代码，遵守了 OCP 原则



#### 观察者模式在JDK应用的源码分析

**JDK 的 Observable 类就使用了观察者模式**

```java
public class Observable {
    private boolean changed = false;
    private Vector<Observer> obs;

    /** Construct an Observable with zero Observers. */

    public Observable() {
        obs = new Vector<>();
    }
}






public interface Observer {
  void update(Observable o, Object arg); 
}
```

{{< admonition question Observable 中角色及职责 >}}

- **Observable** 的作用和地位等价于我们前面讲过的 Subject
- **Observable 是类，不是接口**，类中已经实现了核心的方法，即管理Observer的方法 add.. delete.. notify..
- Observer 的作用和地位等价于我们前面讲过的Observer，有 update
- Observable 和 Observer 的使用方法和前面讲过的一样，只是 Observable 是类，通过**继承**来实现观察者模式

{{< /admonition >}}



<br>

### 中介者模式

#### 智能家庭管理系统

项目需求：

1. 智能家庭包括各种设备，闹钟、咖啡机、电视机、窗帘等
2. 主人要看电视时，各个设备可以协同工作，自动完成看电视的准备工作，比如流程为：闹钟响起 -> 咖啡机开始做咖啡 -> 窗帘自动落下 -> 电视机开始播放

#### 传统方案

![传统设计方案类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BC%A0%E7%BB%9F%E8%AE%BE%E8%AE%A1%E6%96%B9%E6%A1%88%E7%B1%BB%E5%9B%BE.png "传统设计方案类图")

##### 传统设计方案问题分析

1. 当各电器对象有多种状态改变时，相互之间的调用关系会比较复杂
2. 各个电器对象彼此联系，你中有我，我中有你，不利于松耦合
3. 各个电器对象之间所传递的消息（参数），容易混乱
4. 当系统增加一个新的电器对象时，或者执行流程改变时，代码的可维护性、扩展性都不理想 -> 这时可以考虑中介者模式

#### 中介者模式

##### 基本介绍

1. 中介者模式（Mediator Pattern），用一个中介者对象封装一系列的对象交互。中介者使各个对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互
2. 中介者模式属于行为型模式，使代码易于维护
3. 比如 MVC 模式，C（Controller 控制器）是 M（Model 模型）和 V（View 视图）的中介者，在前后端交互时起到了中间人的作用

##### 中介者模式原理类图

![中介者模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png "中介者模式原理类图")

{{< admonition question 中介者模式中角色及职责 >}}

- **Mediator** 就是抽象中介者，定义了同事对象到中介者对象的接口
- **Colleague** 是抽象同事类
- **ConcreteMediator** 具体的中介者对象，实现抽象方法，它需要知道所有的具体同事类，即以一个集合来管理HashMap，并接受某个同事对象消息，完成相应的任务
- **ConcreteColleague** 具体的同事类，会有很多，每个同事只知道自己的行为，而不了解其他同事类的行为（方法），但是它们都依赖中介者对象

{{< /admonition >}}

##### 中介者模式应用实例

![中介者模式解决智能家庭管理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F%E8%A7%A3%E5%86%B3%E6%99%BA%E8%83%BD%E5%AE%B6%E5%BA%AD%E7%AE%A1%E7%90%86%E7%B1%BB%E5%9B%BE.png "中介者模式解决智能家庭管理类图")

```java
public abstract class Mediator {
	//将给中介者对象，加入到集合中
	public abstract void Register(String colleagueName, Colleague colleague);

	//接收消息, 具体的同事对象发出
	public abstract void GetMessage(int stateChange, String colleagueName);

	public abstract void SendMessage();
}







//同事抽象类
public abstract class Colleague {
	private Mediator mediator;
	public String name;

	public Colleague(Mediator mediator, String name) {

		this.mediator = mediator;
		this.name = name;

	}

	public Mediator GetMediator() {
		return this.mediator;
	}

	public abstract void SendMessage(int stateChange);
}







//具体的同事类
public class Alarm extends Colleague {

	//构造器
	public Alarm(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		//在创建Alarm 同事对象时，将自己放入到ConcreteMediator 对象中[集合]
		mediator.Register(name, this);
	}

	public void SendAlarm(int stateChange) {
		SendMessage(stateChange);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		//调用的中介者对象的getMessage
		this.GetMediator().GetMessage(stateChange, this.name);
	}

}







public class TV extends Colleague {

	public TV(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void StartTv() {
		// TODO Auto-generated method stub
		System.out.println("It's time to StartTv!");
	}

	public void StopTv() {
		// TODO Auto-generated method stub
		System.out.println("StopTv!");
	}
}







public class Curtains extends Colleague {

	public Curtains(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void UpCurtains() {
		System.out.println("I am holding Up Curtains!");
	}

}






import java.util.HashMap;

//具体的中介者类
public class ConcreteMediator extends Mediator {
	//集合，放入所有的同事对象
	private HashMap<String, Colleague> colleagueMap;
	private HashMap<String, String> interMap;

	public ConcreteMediator() {
		colleagueMap = new HashMap<String, Colleague>();
		interMap = new HashMap<String, String>();
	}

	@Override
	public void Register(String colleagueName, Colleague colleague) {
		// TODO Auto-generated method stub
		colleagueMap.put(colleagueName, colleague);

		// TODO Auto-generated method stub

		if (colleague instanceof Alarm) {
			interMap.put("Alarm", colleagueName);
		} else if (colleague instanceof CoffeeMachine) {
			interMap.put("CoffeeMachine", colleagueName);
		} else if (colleague instanceof TV) {
			interMap.put("TV", colleagueName);
		} else if (colleague instanceof Curtains) {
			interMap.put("Curtains", colleagueName);
		}

	}

	//具体中介者的核心方法
	//1. 根据得到消息，完成对应任务
	//2. 中介者在这个方法，协调各个具体的同事对象，完成任务
	@Override
	public void GetMessage(int stateChange, String colleagueName) {
		// TODO Auto-generated method stub

		//处理闹钟发出的消息
		if (colleagueMap.get(colleagueName) instanceof Alarm) {
			if (stateChange == 0) {
				((CoffeeMachine) (colleagueMap.get(interMap
						.get("CoffeeMachine")))).StartCoffee();
				((TV) (colleagueMap.get(interMap.get("TV")))).StartTv();
			} else if (stateChange == 1) {
				((TV) (colleagueMap.get(interMap.get("TV")))).StopTv();
			}

		} else if (colleagueMap.get(colleagueName) instanceof CoffeeMachine) {
			((Curtains) (colleagueMap.get(interMap.get("Curtains"))))
					.UpCurtains();

		} else if (colleagueMap.get(colleagueName) instanceof TV) {//如果TV发现消息

		} else if (colleagueMap.get(colleagueName) instanceof Curtains) {
			//如果是以窗帘发出的消息，这里处理...
		}

	}

	@Override
	public void SendMessage() {
		// TODO Auto-generated method stub

	}

}








public class CoffeeMachine extends Colleague {

	public CoffeeMachine(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void StartCoffee() {
		System.out.println("It's time to startcoffee!");
	}

	public void FinishCoffee() {

		System.out.println("After 5 minutes!");
		System.out.println("Coffee is ok!");
		SendMessage(0);
	}
}








public class ClientTest {

	public static void main(String[] args) {
		//创建一个中介者对象
		Mediator mediator = new ConcreteMediator();
		
		//创建Alarm 并且加入到  ConcreteMediator 对象的HashMap
		Alarm alarm = new Alarm(mediator, "alarm");
		
		//创建了CoffeeMachine 对象，并  且加入到  ConcreteMediator 对象的HashMap
		CoffeeMachine coffeeMachine = new CoffeeMachine(mediator,
				"coffeeMachine");
		
		//创建 Curtains , 并  且加入到  ConcreteMediator 对象的HashMap
		Curtains curtains = new Curtains(mediator, "curtains");
		TV tV = new TV(mediator, "TV");
		
		//让闹钟发出消息
		alarm.SendAlarm(0);
		coffeeMachine.FinishCoffee();
		alarm.SendAlarm(1);
	}

}
```

#### 中介者模式注意事项和细节

{{< admonition tip 中介者模式 >}}

1. 多个类相互耦合，会形成网状结构，使用中介者模式将**网状结构分离为星型结构**，进行解耦
2. **减少类之间依赖**，降低了耦合，符合**迪米特原则**
3. 中介者承担了较多的责任，一旦中介者出现了问题，整个系统就会受到影响
4. 如果设计不当，中介者对象本身变得过于复杂，这点在实际使用时，要特别注意

{{< /admonition >}}



<br>

### 备忘录模式

#### 游戏角色状态恢复问题

游戏角色有攻击力和防御力，在大战Boss前保存自身的状态（攻击力和防御力），当大战Boss后攻击力和防御力下降，从备忘录对象恢复到大战前的状态

#### 传统方案

![传统的设计方案（类图）](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BC%A0%E7%BB%9F%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%96%B9%E6%A1%88%EF%BC%88%E7%B1%BB%E5%9B%BE%EF%BC%89.png "传统的设计方案（类图）")

##### 传统方案的问题分析

1. 一个对象，就对应一个保存对象状态的对象，这样当我们游戏的对象很多时，不利于管理，开销也很大
2. 传统的方式是简单地做备份，new出另一个对象出来，再把需要备份的数据放到这个新对象，但这就暴露了对象内部的细节
3. 解决方案 -> 备忘录模式



<br>

#### 备忘录模式

##### 基本介绍

1. **备忘录模式（Memento Pattern）**在不破坏封装性的前提下，捕获一个对象的**内部状态**，并在该**对象之外**保存这个状态。这样以后就可将该对象恢复到原先保存的状态
2. 可以这样理解备忘录模式：显示生活中的备忘录是用来记录某些要去做的事情，或者是记录已经达到的共同意见的事情，以防忘记了。而在软件层面，备忘录模式有着相同的含义，**备忘录对象主要用来记录一个对象的某种状态**，或者某些数据，当要做**回退**时，**可以从备忘录对象里获取原来的数据进行恢复操作**
3. 备忘录模式属于**行为型模式**

##### 备忘录模式原理

![备忘录模式原理类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F%E5%8E%9F%E7%90%86%E7%B1%BB%E5%9B%BE.png "备忘录模式原理类图")

{{< admonition question 备忘录模式中角色及职责 >}}

- **Originator**：**对象**（需要保存状态的对象）
- **Memento**：**备忘录对象**，负责保存好记录。即 Originator 内部状态
- **Caretaker**：**守护者对象**，负责保存多个备忘录对象，使用集合管理，提高效率
- **说明**：如果希望保存多个 Orifginator 对象的不同时间的状态，也可以，只需要 **HashMap<String,集合>**

{{< /admonition >}}

针对上面的备忘录模式原理结构图，我们使用代码简单实现，注意体会体现出 Caretaker 可以保存多个备忘录对象，方便管理，提高效率

```java
public class Originator {

	private String state;//状态信息

	public String getState() {
		return state;
	}

	public void setState(String state) {
		this.state = state;
	}
	
	//编写一个方法，可以保存一个状态对象 Memento
	//因此编写一个方法，返回 Memento
	public Memento saveStateMemento() {
		return new Memento(state);
	}
	
	//通过备忘录对象，恢复状态
	public void getStateFromMemento(Memento memento) {
		state = memento.getState();
	}
}






public class Memento {
	private String state;

	//构造器
	public Memento(String state) {
		super();
		this.state = state;
	}

	public String getState() {
		return state;
	}
	
}







import java.util.ArrayList;
import java.util.List;

public class Caretaker {
	
	//在List 集合中会有很多的备忘录对象
	private List<Memento> mementoList = new ArrayList<Memento>();
	
	public void add(Memento memento) {
		mementoList.add(memento);
	}
	
	//获取到第index个Originator 的 备忘录对象(即保存状态)
	public Memento get(int index) {
		return mementoList.get(index);
	}
}







import java.util.ArrayList;
import java.util.HashMap;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Originator originator = new Originator();
		Caretaker caretaker = new Caretaker();
		
		originator.setState(" 状态#1 攻击力 100 ");
		
		//保存了当前的状态
		caretaker.add(originator.saveStateMemento());
		
		originator.setState(" 状态#2 攻击力 80 ");
		
		caretaker.add(originator.saveStateMemento());
		
		originator.setState(" 状态#3 攻击力 50 ");
		caretaker.add(originator.saveStateMemento());
		
		
		
		System.out.println("当前的状态是 =" + originator.getState());
		
		//希望得到状态 1, 将 originator 恢复到状态1
		
		originator.getStateFromMemento(caretaker.get(0));
		System.out.println("恢复到状态1 , 当前的状态是");
		System.out.println("当前的状态是 =" + originator.getState());
		
		
		
	}

}
```

##### 备忘录模式应用实例

![备忘录模式应用实例类图](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B%E7%B1%BB%E5%9B%BE.png "备忘录模式应用实例类图")

```java
public class Memento {

	//攻击力
	private int vit;
	//防御力
	private int def;
	public Memento(int vit, int def) {
		super();
		this.vit = vit;
		this.def = def;
	}
	public int getVit() {
		return vit;
	}
	public void setVit(int vit) {
		this.vit = vit;
	}
	public int getDef() {
		return def;
	}
	public void setDef(int def) {
		this.def = def;
	}
}







public class GameRole {

	private int vit;
	private int def;
	
	//创建Memento ,即根据当前的状态得到Memento
	public Memento createMemento() {
		return new Memento(vit, def);
	}
	
	//从备忘录对象，恢复GameRole的状态
	public void recoverGameRoleFromMemento(Memento memento) {
		this.vit = memento.getVit();
		this.def = memento.getDef();
	}
	
	//显示当前游戏角色的状态
	public void display() {
		System.out.println("游戏角色当前的攻击力：" + this.vit + " 防御力: " + this.def);
	}

	public int getVit() {
		return vit;
	}

	public void setVit(int vit) {
		this.vit = vit;
	}

	public int getDef() {
		return def;
	}

	public void setDef(int def) {
		this.def = def;
	}
}






import java.util.ArrayList;
import java.util.HashMap;

//守护者对象, 保存游戏角色的状态
public class Caretaker {

	//如果只保存一次状态
	private Memento  memento;
	//对GameRole 保存多次状态
	//private ArrayList<Memento> mementos;
	//对多个游戏角色保存多个状态
	//private HashMap<String, ArrayList<Memento>> rolesMementos;

	public Memento getMemento() {
		return memento;
	}

	public void setMemento(Memento memento) {
		this.memento = memento;
	}
}






public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建游戏角色
		GameRole gameRole = new GameRole();
		gameRole.setVit(100);
		gameRole.setDef(100);
		
		System.out.println("和boss大战前的状态");
		gameRole.display();
		
		//把当前状态保存caretaker
		Caretaker caretaker = new Caretaker();
		caretaker.setMemento(gameRole.createMemento());
		
		System.out.println("和boss大战~~~");
		gameRole.setDef(30);
		gameRole.setVit(30);
		
		gameRole.display();
		
		System.out.println("大战后，使用备忘录对象恢复到站前");
		
		gameRole.recoverGameRoleFromMemento(caretaker.getMemento());
		System.out.println("恢复后的状态");
		gameRole.display();
	}

}
```

#### 备忘录模式注意事项和细节

{{< admonition tip 中介者模式 >}}

1. 给用户提供了一种可以恢复状态的机制，可以使用户能够比较方便地回到某个历史的状态
2. 实现了信息的封装，使得用户不需要关心状态的保存细节
3. 如果类的成员变量变多，势必会占用比较大的资源，而且每一次保存都会消耗一定的内存，这个需要注意
4. 适用场景：
    - 后悔药
    - 打开游戏的存档
    - Windows 里的 ctri + z
    - IE 中的回退
    - 数据库的事务管理

5. 为了节约内存，备忘录模式可以和原型模式配合使用

{{< /admonition >}}



<br>

### 解释器模式



















<br>

**未完待续···**






