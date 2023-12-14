# MySQL 索引


<!--more-->

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

## 索引介绍

### 什么是MySQL的索引

> MySQL官方对于索引的定义：**索引是帮助MySQL高效获取数据的数据结构。**

MySQL在存储数据之外，数据库系统中还维护着满足特定查找算法的数据结构，这些数据结构以某种引用(指向)表中的数据，这样我们就可以通过数据结构上实现的高级查找算法来快速找到我们想要的数据。而这种数据结构就是**索引**。

简单理解为“**排好序的可以快速查找数据的数据结构**”。

<br>

### 索引数据结构

下图展示的就是一种可能的二叉树的索引方式：

![二叉树索引方式](https://cdn.jsdelivr.net/gh/Turbo-King/images/image-20231214100639030.png "二叉树索引方式")

**二叉树数据结构的弊端**：当极端情况下，数据递增插入时，会一直向右插入，形成链表，查询效率会降低。



> MySQL中常用的索引数据结构有BTree索引（Myisam 普通索引），B+Tree索引（Innodb 普通索引）,Hash索引（Memory 存储引擎）等等。

<br>

### 索引优势

提高数据检索的效率，降低数据库的IO成本

通过索引对数据进行排序，降低数据排序的成本，降低了CPU的消耗。

<br>

### 索引优势

索引实际上也是一张表，保存了主键和索引的字段，并且指向实体表的记录，索引索引也是需要占用空间的。在索引大大提高查询速度的同时，却会降低表的更新速度，在对表进行数据增删改的同时，MySQL不仅要更新数据，还需要保存一下索引文件。每次更新添加了的索引列的字段，都会去调整因为更新带来的减值变化后的索引的信息。

<br>

### 索引使用场景

**下列情况需要创建索引**

1. 主键自动建立唯一索引
2. 频繁作为查询条件的字段应该创建索引（where 后面的语句）
3. 查询中与其它表关联的字段，外键关系建立索引
4. 多字段查询下倾向创建组合索引
5. 查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度
6. 查询中统计或者分组字段

<br>

**下列情况不推荐建立索引**

1. 表记录太少
2. 经常增删改的表
3. Where 条件里用不到的字段不建立索引

<br>

### 索引分类

#### 主键索引

1. 表中的列设定为主键后，数据库会自动建立主键索引

2. 单独创建键和删除主键索引语法：

    - 创建主键索引语法

    ```mysql
    alter table 表名 add primary key (字段);
    ```

    - 删除主键索引语法

    ```mysql
    alter table 表名 drop primary key;
    ```

<br>

#### 唯一索引

1. 表中的列创建了唯一约束时，数据库会自动建立唯一索引。

2. 单独创建和删除唯一索引语法：

    - 创建唯一索引语法

    ```mysql
    alter table 表名 add unique 索引名(字段);
    或
    create unique index 索引名 on 表名(字段);
    ```

    - 删除唯一索引语法

    ```mysql
    drop index 索引名 on 表名;
    ```

<br>

#### 单值索引

即一个索引只包含单个列，一个表可以有多个单值索引。

1. 建表时可随表一起建立单值索引

2. 单独创建和删除单值索引：

    - 创建单值索引

    ```mysql
    create index 索引名 on 表名(字段);
    或
    alter table 表名 add index 索引名(字段);
    ```

    - 删除单值索引

    ```mysql
    drop index 索引名 on 表名;
    ```

<br>

#### 复合索引

即一个索引包含多个列。

1. 建表时可随表一起建立复合索引

2. 单独创建和删除复合索引：

    - 创建复合索引

    ```mysql
    create index 索引名 on 表名(字段1，字段2);
    ```

    - 删除复合索引

    ```mysql
    drop index 索引名 on 表名;
    ```

<br>

## 性能分析

### MySQL常见瓶颈

SQL中对大量数据进行比较、关联、排序、分组时，产生CPU瓶颈。

实例内存满足不了缓存数据或排序等需要，导致产生大量的物理IO。查询数据时扫描过多数据行，导致查询效率低。

<br>

### Explain分析




















