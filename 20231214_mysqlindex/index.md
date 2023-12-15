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

    ```sql
    alter table 表名 add primary key (字段);
    ```

    - 删除主键索引语法

    ```sql
    alter table 表名 drop primary key;
    ```

<br>

#### 唯一索引

1. 表中的列创建了唯一约束时，数据库会自动建立唯一索引。

2. 单独创建和删除唯一索引语法：

    - 创建唯一索引语法

    ```sql
    alter table 表名 add unique 索引名(字段);
    或
    create unique index 索引名 on 表名(字段);
    ```

    - 删除唯一索引语法

    ```sql
    drop index 索引名 on 表名;
    ```

<br>

#### 单值索引

即一个索引只包含单个列，一个表可以有多个单值索引。

1. 建表时可随表一起建立单值索引

2. 单独创建和删除单值索引：

    - 创建单值索引

    ```sql
    create index 索引名 on 表名(字段);
    或
    alter table 表名 add index 索引名(字段);
    ```

    - 删除单值索引

    ```sql
    drop index 索引名 on 表名;
    ```

<br>

#### 复合索引

即一个索引包含多个列。

1. 建表时可随表一起建立复合索引

2. 单独创建和删除复合索引：

    - 创建复合索引

    ```sql
    create index 索引名 on 表名(字段1，字段2);
    ```

    - 删除复合索引

    ```sql
    drop index 索引名 on 表名;
    ```

<br>

## 性能分析

### MySQL常见瓶颈

SQL中对大量数据进行比较、关联、排序、分组时，产生CPU瓶颈。

实例内存满足不了缓存数据或排序等需要，导致产生大量的物理IO。查询数据时扫描过多数据行，导致查询效率低。

<br>

### Explain分析

使用 **Explain** 关键字可以模拟优化器执行 SQL 查询语句，从而知道 MySQL 是如何处理 SQL 语句的。可以用来分析查询语句或是表的结构的性能瓶颈。作用如下：

1. 表的读取顺序
2. 哪些索引可以使用
3. 数据读取操作的操作类型
4. 哪些索引被实际使用
5. 表之间的引用
6. 每张表有多少行被优化器查询

**Explain** 关键字使用起来比较简单：**explain + SQL语句**

![explain执行结果参数](https://cdn.jsdelivr.net/gh/Turbo-King/images/image-20231214160254115.png "explain执行结果参数")

### Explain 重要字段名

#### id

select 查询的系列号，表示查询中执行 select 子句或操作表的顺序。

id 相同时，执行顺序由上至下。

id 不同时，如果为子查询，id 的序号会递增，id 值越大优先级越高，则先被执行。

id 相同和不同都存在，id 相同的可以理解为一组，从上往下顺序执行，所有组中，id值越大，优先级越高越先执行。

<br>

#### select_type

查询的类型，常见值有：

**SIMPLE**：简单的 select 查询，查询中不包含子查询或者 UNION

```sql

```

**PRIMARY**：查询中若包含任何复杂的子部分，最外层查询则被标记为 Primary。

```sql

```

**DERIVED**：在 FROM 列表中包含的子查询被标记为 DERIVED（衍生）MySQL 会递归执行这些子查询，把结果放在临时表里。

```sql

```

**SUBQUERY**：在 SELECT 或 WHERE 列表中包含了子查询。

```sql

```

<br>

#### table

显示这一行的数据是来自哪张表的。

<br>

#### type

访问类型排序：

![访问类型](https://cdn.jsdelivr.net/gh/Turbo-King/images/image-20231214192611646.png "访问类型")

**system**:表中只有一行记录（等于系统表），这是 const 类型的特性，平时不会出现，这个也可以忽略不计。

```sql

```

**const**:表示通过索引一次就找到了，const 用于比较 **primary key** 或者 **unique** 索引。因为只匹配一行数据，所以很快，如将主键置于 where 列表中，MySQL 就能将该查询转换为一个常量。

```sql
sql
```

**eq_ref**:唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见主键或唯一索引扫描。

```sql
sql
```

**ref**:非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，它返回所有匹配某个单独值的行，然后，它可能会找到多个符合条件的行，所以它应该属于查找和扫描的混合体。

```sql
sql
```

**range**:只检索给定范围的行，使用一个索引来选择行。key 列显示使用了哪个索引，一般就是在你的 where 语句中出现了 between、<、>、in 等的查询这种范围扫描索引扫描比全表扫描要好，因为它只需要开始于索引的某一点，而结束语另一点，不用扫描全部索引。

```sql
sql
```

**index**:Full Index Scan，index 与 All 区别为 index 类型只遍历索引树。这通常比 All 快，因为索引文件通常比数据文件小，也就是说虽然 all 和 index 都是读全表，但 index 是从索引中读取的，而 all 是从硬盘中读的。

```sql
sql
```

**All**:Full Table Scan，将遍历全表以找到匹配的行。

```sql
sql
```

> 从最好到最差依次是：**system > const >  eq_ref > ref > range > All** 一般来说，最好保证查询能够达到 **range** 级别，最好达到 **ref**。

<br>

#### possible_keys

显示可能应用在这张表中的索引，一个或多个。查询涉及到的字段上如果存在索引，则相应索引将会被列出来，但不一定会被查询实际使用上。

<br>

#### key

查询中实际使用的索引，如果为 NULL，则没有使用索引。

<br>

#### key_len

表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。在不损失精确性的情况下，**长度越短越好**。

<br>

#### ref

显示索引的哪一列被使用了。哪些列或常量被用于查找索引列上的值。

<br>

#### rows

rows 列显示 MySQL 认为它执行查询时必须检查的行数。一般越少越好。

<br>

#### extra

一些常见的重要的额外信息：

- **Using filesort**：MySQL 无法利用索引完成的排序操作称之为“文件排序”。
- **Using temporary**：MySQL 在对查询结果排序时使用临时表，常见排序 **order by** 和分组查询 **group by**。
- **Using index**：表示索引被用来执行索引键值的查找，避免访问了表的数据行，效率还不错。
- **Using where**：表示使用了 **where** 过滤。

<br>

## 查询优化

### 索引失效

1. **最佳左前缀法则**：如果索引了多列，要遵循最左前缀法则，指的是查询从索引的最左前列开始并且不跳过索引中的列。
2. **不在索引上做任何计算、函数操作**，会导致索引失效转而转向全表扫描。
3. 存储引擎不能使用索引中范围条件右边的列。
4. MySQL 在使用不等于时无法使用索引会导致全表扫描。
5. **is null** 可以使用索引，但是 **is not null** 无法使用索引。
6. **like** 以通配符开头会使索引失效导致全表扫描。
7. 字符串不加单引号索引会失效。
8. 使用 **or** 连接时索引失效。

<br>

**实战演示**

假设 **index(a,b,c)** 复合索引：注意 or 不会生效，and 会自动调整顺序为最左前列。

|                      **where语句**                      |         **索引是否被使用**          |
| :-----------------------------------------------------: | :---------------------------------: |
|                       where a = 3                       |            Yes，使用到a             |
|                  where a = 3 and b = 5                  |          Yes，使用到 a，b           |
|             where a = 3 and b = 4 and c = 5             |         Yes，使用到 a，b，c         |
| Where b = 4 或者 where b = 4 and c = 4 或者 where c = 5 |                 No                  |
|                  where a = 3 and c = 5                  | **Yes，只用到 a，key_len 会小一些** |
|             where a = 3 and b > 4 and c = 5             |        **Yes，使用到 a，b**         |
|         where a = 3 and b like 'kk%' and c = 5          |         Yes，使用到 a，b，c         |
|         where a = 3 and b like '%kk' and c = 5          |          **Yes，只用到 a**          |
|         where a = 3 and b like '%kk%' and c = 5         |          **Yes，只用到 a**          |
|        where a = 3 and b like 'k%kk%' and c = 5         |         Yes，使用到 a，b，c         |

{{< admonition tip 索引使用建议 >}}

1. 对于单值索引，尽量选择针对当前查询字段过滤性更好的索引。
2. 对于组合索引，当前 where 查询中过滤性更好的字段在索引字段顺序中位置越靠前越好。
3. 对于组合索引，尽量选择能够包含当前查询中 where 子句中更多字段的索引。
4. 尽可能通过分析统计信息和调整 query 的写法来达到选择合适索引的目的。

{{< /admonition >}}

<br>

### 单表查询优化



<br>

### 关联查询优化



<br>

### 排序优化



<br>

### 分组优化



<br>

## 慢查询日志

### 介绍

MySQL 的慢查询日志是 MySQL 提供的一种日志记录，它用来记录在 MySQL 中响应时间超过设定阀值的语句，具体指运行时间超过 long_query_time 值的 SQL ，则会被记录到慢查询日志中。可以由它来查看哪些 SQL 超出了我们最大忍耐时间值。

<br>

### 慢查询日志使用

默认情况下，MySQL 数据库咩有开启慢查询日志，需要手动设置参数。

**查看是否开启慢查询日志：**

```sql
show variables like '%slow_query_log%';
```

**开启日志：**

```sql
set global show_query_log = 1;
```

**设置时间阀值：**

```sql
set global long_query_time = 1;
```

**查看设置的时间阀值：**

```sql
show variables like 'long_query_time%';
```

查看超时的 sql 记录日志：MySQL 的数据文件夹下 '\Data\设备名称-slow.log'

>注意：非调优场景下，一般不建议开启改参数，慢查询日志将日志记录写入文件，开启慢查询日志会或多或少带来一定的性能影响。
























