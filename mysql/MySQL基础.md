### MySQL 基础



#### MySQL 的数据类型：

整数类型（zerfill 填充）：TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT

属性：UNSIGNED (非负)

长度：可以为整数类型指定宽度，例如：int(11) ，对大多数应用来说没有意义的，它不会限制值得合法范围，只会影响显示字符的个数。



实数类型：

FLOAT、DOUBLE、DECIMAL

DECIMAL 可以存储比 BIGINT 还大的整数；可以用于存储精确的小数

FLOAT和DOUBLE类型支持使用标准的浮点进行近似计算



字符串类型：

VARCHAR、CHAR、TEXT、BLOB



VARCHAR 用于存储可变长度字符串，它比定长类型更节省空间

VARCHAR 使用1或2个额外字节记录字符串的长度，列长度小于255字节，使用1个字节代表，否则用2个

VARCHAR 长度，如果存储内容超出指定长度，会被截断



CHAR 是定长的，根据定义的字符串长度分配足够的空间

CHAR 会根据需要采用空格进行填充以方便比较

CHAR 适合存储很短的字符串，或者所有值都接近同一个长度

CHAR 长度，超出设定的长度，会被截断



对于经常变更的数据，CHAR 比 VARCHAR 更好，CHAR 不容易产生碎片

对于非常短的列，CHAR 比VARCAHR 在存储空间上更有效率只分配真正的空间，更长的列会消耗更多的内存



字符串类型：尽量避免使用BLOB\TEXT类型，查询会使用临时表，导致严重的性能开销。



枚举：

有时可以使用枚举代替常用的字符串类型，把不重复的集合存储成一个个预定义的集合。非常紧凑，把压缩到一个或者两个字节，内部存储的是整数。

尽量避免使用数字类型作为ENUM枚举的常量，易混乱；排序是按照内部存储的整数进行排序

枚举表会使表大大减小



日期和时间类型:

尽量使用TIMESTAMP，比如DATETIME空间效率高

用整数保存时间戳的格式通常不好处理

如果需要存储微秒，可以使用 bigint 存储



#### 列属性：

auto_increment、default、not null、zerofill



#### 常见操作

MySQL 的连接和关闭：mysql -u -p -h -P

其他：\G、\c、\q、\s、\h、\d

\G 格式化处理打印

\c 取消当前 mysql 命令

\q 退出

\s 显示数据状态

\h 帮助信息

\d 改变执行服务



#### MySQL 数据表引擎

InnoDB 表引擎(行级锁)：

默认事务型引擎，最重要最广泛的存储引擎，性能非常优秀

数据存储在共享表空间，可以通过配置分开（多个表、索引都在一个空间）

对主键查询的性能高于其他类型的存储引擎

内部做了很多优化，从磁盘读取数据时自动在内存构建 hash 索引，插入数据时自动构建插入缓冲区

通过一些机制和工具支持真正的热备份

支持崩溃后的安全恢复 支持行级锁、支持外键



MyISAM 表引擎(表锁)：

5.1 版本前，MyISAM 是默认的存储引擎

拥有全文索引、压缩、空间函数

不支持事务和行级锁，不支持崩溃后的安全恢复

表存储在两个文件，MYD 和 MYI (数据和索引)

设计简单，某些场景下性能更好



其他表引擎：

Archive、Blackhole、CSV、Memory



#### MySQL 锁机制

表锁是日常开发当中常见的问题，当多个查询同一时刻进行修改数据时，就会产生并发控制的问题。

共享锁和排他锁，其实就是读锁和写锁

读锁：共享的，不堵塞，多个用户可以同时读一个资源，互不干扰

写锁：排他的，一个写锁会阻塞其他的写锁和读锁，这样可以只允许 一个进行写入，防止其他用户读取正在写入的资源

锁粒度：

表锁，系统性能开销最小，会锁定整张表，MyISAM 使用表锁

行锁，最大程度地支持并发处理，但是也带来了最大的锁开销，InnDB 实现行级锁



#### 事务处理

MySQL 提供事务处理的表，InnoDB

服务器层不管事务，由下层的引擎实现，所以同一个事务中，使用多种存储引擎不靠谱

在非事务的表上执行事务操作 MySQL 不会发出提醒，也不会报错



#### 存储过程

 为以后的使用而保存的一条或者多条 MySQL 语句的集合

存储过程就是业务逻辑和流程的集合

可以在存储过程中创建表，更新数据，删除数据等



使用场景：

通过把处理封装在容易使用的单元中，简化复杂的操作

保证数据的一致性、简化对变动的管理



#### 触发器

提供给程序员和数据分析员来保证数据完整性的一种方法，他是与表事务相关的特殊的存储过程



使用场景：可通过数据库中的相关表实现级联更改

实时监控某张表中的某个字段的更改而需要作出相应的处理

某些业务编号的生成等

滥用会造成数据库及应用程序的维护困难

 

