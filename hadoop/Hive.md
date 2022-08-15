# [Hive笔记之数据库操作](https://www.cnblogs.com/cc11001100/p/9033930.html)

## 创建数据库

hive创建数据库的最简单写法和mysql差不多：

```sql
create database foo;
```

仅当名为foo的数据库当前不存在时才创建：

```sql
create database if not exists foo;
```

创建数据库时指定位置，这个位置一般是在hdfs上的位置：

```sql
create database foo location '/db/foo'; 
```

查看已经创建的数据库：

```sql
show databases;
```

使用通配符查看foo开头的数据库：

```sql
show databases like 'foo.*'; 
```

查看创建数据库的语句：

```sql
show create database foo;
```

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231556904-1441546017.png)

hive为每一个数据库创建一个目录，这个数据库中的表将会以子目录的形式放在这个数据库目录下

有一个例外就是default数据库中的表，default数据库没有自己的目录，所以是直接放在/user/hive/warehouse下面的：

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231558642-1631513101.png) 

同样的，当创建数据库如果没有指定存储位置，默认就是在/user/hive/warehouse/下的：

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513232315125-808689631.png)

## 数据库描述信息

在创建数据库时可以指定描述性信息：

```sql
create database foo comment 'this is foo database';
```

通过describe database可以查看到数据库的详细信息：

```sql
describe databaseb foo;
```

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231603344-105531471.png)

## 数据库键值对信息

数据库可以有一些描述性的键值对信息，在创建时添加：

```sql
create database foo with dbproperties ('own'='cc', 'day'='20180120');
```

查看数据库的键值对信息：

```sql
describe database extended foo;
```

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231604000-638707555.png)

要修改数据库的键值对信息：

```sql
alter database foo set dbproperties ('k1'='v1', 'k2'='v2');
```

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231606258-431292897.png)

## 删除数据库

```sql
drop database if exists foo;
```

注意：

默认情况下是不允许直接删除一个有表的数据库的：

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231606892-1655354286.png)

删除一个有表的数据库有两种办法：

\1. 先把表删干净，再删库。

\2. 删库时在后面加上cascade，表示级联删除此数据库下的所有表：

```sql
drop database if exists foo cascade;
```

sq 

## prompt显示当前数据库名称

如果在一个数据库很多的环境下工作，需要 在不同的库之间切来切去（手动敲查询的时候全使用dbName.table可不是什么好主意…），可能一不小心就忘记自己当前在哪个数据库下了，可以通过设置一个属性改变当前的命令提示符，更专业的说法是prompt（用过CLI界面的应该对这个概念很熟悉），通过设置hive.cli.print.current.db属性可以在hive cli中显示当前数据库的名称，比如当前的数据库名称是foo：

```sql
set hive.cli.print.current.db=true
```

![image](https://images2018.cnblogs.com/blog/784924/201805/784924-20180513231607210-1257024068.png)