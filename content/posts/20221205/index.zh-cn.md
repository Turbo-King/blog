---
weight: 5
title: "设计模式"
date: 2022-12-05T08:53:36+08:00
draft: false
author: "Turbo-King"
authorLink: "https://turbo-king.github.io/"
description: "设计模式讲解"
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["Java","coding","principle"]
categories: ["Markdown"]

lightgallery: true
---

# 浅谈设计模式

<!--more-->

### 前言
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

> 编写软件过程中，程序员面临着来自**耦合性**，**内聚性**以及**可维护性**，**可扩展性**，**重用性**，**灵活性** 等多方面的挑战，设计模式是为了让程序(软件)，具有更好：
>
> 1. **代码重用性**（即：相同的代码，不用多次编写）
> 2. **可读性**（即：编程规范性，便于其他程序员的阅读与理解）
> 3. **可扩展性**（即：当需要增加新的功能时，非常方便，称为可维护性）
> 4. **可靠性**（即：当我们增加新的功能后，对原来的功能没有影响）
> 5. 使程序呈现**高内聚，低耦合**的特性

设计模式包含了面向对象的精髓：“懂了设计模式，你就懂了面向对象分析和设计（OOA/D）的精要”。

Scott Mayers 在其巨著《Effective C++》就曾经说过：C++老手和 C++新手的区别就是前者手背上有很多伤疤。



### 设计模式七大原则

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

![单一职责.png](https://cdn.jsdelivr.net/gh/Turbo-King/images/单一职责.png.jpeg "单一职责原则")



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

![截屏2022-12-05 14.40.01](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2022-12-05%2014.40.01.png "接口隔离原则-未拆分")

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

![截屏2022-12-05 14.56.50](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2022-12-05%2014.56.50.png "接口隔离原则-拆分")

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
