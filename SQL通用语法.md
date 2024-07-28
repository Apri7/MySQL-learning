# SQL通用语法
***
## 通用语法
1. SQL语句单行或多行书写，以`;`结尾
2. SQL语句可以使用空格或缩进来增强语句的可读性
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
4. 注释：单行用`--注释内容` 或 `#注释内容`； 多行用`/*...*/`
## 分类
1. DDL(Data Definition Language) 数据说明语言，用来定义数据库对象（数据库、表、字段）。
2. DML(Data Manipulation Language) 数据操作语言，用来对数据库表中的数据进行增删改。
3. DQL(Data Query Language) 数据查询语言，用来查询数据库中表的记录。
4. DCL(Data Control Language) 数据控制语言，用来管理数据库用户、控制数据库的访问权限。
***
## DDL-数据说明语言
*方括号里的内容可写可不写*

**一、 数据库操作**
1. 查询：
   1. 查询所有数据库：`SHOW DATABASES;`
   2. 查询当前数据库：`SELECT DATABASE();`
2. 创建：`CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];`
3. 删除：`DROP DATABASE [IF EXISTS]数据库名;`
4. 使用：`USE 数据库名;`

**二、表操作**
1. 查询：
   1. 查询当前数据库所有表：`SHOW TABLES;`
   2. 查询表结构：`DESC 表名;`
   3. 查询指定表的建表语句：`SHOW CREATE TABLE 表名;`
2. 创建：
   ```SQL
   create table 表名(
    字段1 字段1类型[COMMENT 字段1注释],
    字段2 字段2类型[COMMENT 字段2注释],
    字段3 字段3类型[COMMENT 字段3注释],
    ......
    字段n 字段n类型[COMMENT 字段n注释] --最后是没有逗号结尾的
   )[COMMENT 表注释];
   ```
   example:
```SQL
  create table emp(
  id int comment '编号',
  workno varchar(10) comment '工号',
  name varchar(10) comment '姓名',
  gender char(1) comment '性别',
  age tinyint unsigned comment '年龄',
  idcard char(18) comment '身份证号',
  entrydate date comment '入职时间'
) comment '员工表';
```

3. 修改：
   1. 添加字段：```ALTER TABLE 表名 ADD 字段名 类型（长度）[COMMENT 注释] [约束];```
   2. 修改：
      1. 修改数据类型 ```ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）;```
      2. 修改字段名和字段类型```ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释][约束];```
   3. 删除字段：```ALTER TABLE 表名 DROP 字段名;```
   4. 修改表名：```ALTER TABLE 表名 RENAME TO 新表名;```
   
4. 删除：
   1. 删除表：```DROP TABLE [IF EXISTS] 表名;```
   2. 删除指定表，并重新创建该表：```TRUNCATE TABLE 表名;```（即初始化该表）

   **三、数据类型**
   （1）数值类型
   | 类型 | 大小 | 有符号（signed）范围 | 无符号（unsigned）范围 | 描述 |
   | :---:| --- |     :---          |     :---            | :--- |
   |TINYINT |1 byte | (-128,127)|(0,255)|小整数值|
   |SMALLINT|2 bytes |... |... | 大整数值 |
   |MEDIUMINT|3 bytes |... |... | 大整数值 |
   |INT 或 INTEGER|4 bytes |... |... | 大整数值 |
   |BIGINT|8 bytes |... |... | 极大整数值 |
   |FLOAT|4 bytes |... |... | 单精度浮点数值 |
   |DOUBLE|4 bytes |... |... | 双精度浮点数值 |
   |DECIMAL | |依赖于M（精度）和D（标度）的值 |依赖于M（精度）和D（标度）的值 |小数值（精确定点数）|   

   精度精度是总的有效数位数，标度是小数点后有几位。
   (2)字符串类型
   | 类型 | 大小 | 描述 |
   | :---:  | :--- | :--- |
   |CHAR| 0-255 bytes | 定长字符串 |
   |VARCHAR| 0-65535 bytes | 变长字符串 |
   |TINYBLOB| 0-255 bytes | 不超过255个字符的二进制数据 |
   |TINYTEXT| 0-255 bytes | 短文本字符串 |
   |BLOB| 0-65535 bytes |二进制形式的长文本数据 |
   |TEXT| 0-65535 bytes | 长文本数据 |
   |MEDIUMBLOB| 0-16777215 bytes | 二进制形式的中等长度文本数据  |
   |MEDIUMTEXT| 0-16777215 bytes | 中等长度文本数据 |
   |LONGBLOB| 0-4294967295 bytes | 二进制形式的极大文本数据 |
   |LONGTEXT| 0-4294967295 bytes | 极大文本数据 |
CHAR(10)表示一个长度为10的字符串，若存储两个字符，则其余八个字符用空格补位；而VARCHAR（10）最大字符存储空间为10，实际字符空间根据实际存储的字符计算。
(3)日期类型
| 类型 | 大小 | 范围 | 格式 | 描述 |
| :--- | :---: | :--- | :---: | :--- |
|DATE | 3 |1000-01-01 至 9999-12-31 | YYYY-MM-DD|日期值 |
|TIME |3 |-838:59:59 - 838:59:59 | HH:MM:SS|时间值或持续时间|
|YEAR|1|1901至2155|YYYY|年份值|
|DATETIME|8|1000-01-01 00：00：00 至 9999-12-31 23：59：59|YYYY-MM-DD HH:MM:SS|混合日期和时间值|
|TIMESSTAMP |4|1970-01-01 00：00：01 至 2038-01-19 03：14：07|YYYY-MM-DD HH:MM:SS|混合日期和时间值、时间戳|
***
## DML-数据操作语言
**一、添加数据**
1. 给指定字段添加数据：```INSERT INTO 表名(字段名1,字段名2,...) VALUES(值1,值2,...);```
2. 给全部字段添加数据：```INSERT INTO 表名 VALUES(值1,值2,...);```
3. 批量添加数据
   1. ```INSERT INTO 表名(字段名1，字段名2，...) VALUES(值1，值2，...),(值1,值2,...),(值1,值2,...);```
   2. ```INSERT INTO 表名 VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...);```  
   
**注意：**
+ 插入数据时，指定的字段顺序与值的顺序一一对应。
+ 字符串和日期型数据应该包含在单引号中。
+ 插入的数据大小在字段的规定范围内。

**二、修改数据**
`UPDATE 表名 SET 字段名1=值1,字段名2=值2,...[WHERE 条件];`
语句没有`WHERE [...]`时，将修改所有列的对应字段。

**三、删除数据**
`DELETE FROM 表名 [WHERE 条件];`
如果语句没有`[WHERE ...]`，同样的将删除整张表的所有数据；  
DELETE 语句不能删除某一个字段的值（可以使用UPDATE）。
***
## DQL-数据查询语言
**一、基本查询**
1. 查询多个字段
   1. `SELECT 字段1,字段2,字段3,...FROM 表名;`
   2. 查询全部字段：`SELECT * FROM 表名;`
2. 查寻某字段并设置别名
   `SELECT 字段1 [[AS] 别名1],字段2[[AS] 别名2] ...FROM 表名;`
3. 查询某字段（不显示重复记录）
    `SELECT DISTINCT 字段列表 FROM 表名;`

**二、条件查询**
1. 语法
   `SELECT 字段列表 FROM 表名 WHERE 条件列表;`
2. 条件
   1. 比较运算符：>、>=、<、<=、=、！=或<>、between...and...、in(...)、like 占位符（模糊匹配，_匹配单个字符，%匹配任意字符）、is null
   2. 逻辑运算符：and或&&、or或||、not或！
   ```SQL
   [例1]查询年龄在20到30岁的员工
       select * from emp where age >= 20 and age <= 30;
       或
       select * from emp where age between 20 and 30;
    [例2]查询年龄为20或30或40的员工信息
         select * from emp where age in(20,30,40);
    [例3]查询姓名为两个字的员工信息
        select * from emp where name like '__';
    [例4]查询身份证号最后一位为x的员工信息
        select * from emp where idcard like '%x';--或用17个下划线代替%
    ```
**三、聚合函数**
1. 介绍：将一列数据作为一个整体，进行纵向计算。
2. 常见聚合函数：
   | 函数 | 功能 |
   | :---: | :---: |
   |count | 统计数量 |
   | max | 最大值 |
   | min | 最小值 |
   | avg | 平均值 |
   | sum | 求和 |
3. 语法：`SELECT 聚合函数(字段列表) FROM 表名;`
   **注意：null值不参与所有聚合函数运算**

**四、分组查询**
1. 语法
  `SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];`
2. where与having区别
   1. 执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
   2. 判断条件不同：where不能对聚合函数进行判断，而having可以。
```SQL
   [例1]根据性别分组，统计男性员工和女性员工的数量。
        select gender, count(*) from emp group by gender;
   [例2]根据性别分组，统计男性员工和女性员工的平均年龄。
        select gender, avg(gender) from emp group by gender; 
   [例3]查询年龄小于45岁的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址。
        select workaddress, count(*) from emp where age < 45 group by workaddress having count(*) >= 3;   
```
**注意：**
+ 执行顺序：where>聚合函数>having。  
+ 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。

**五、排序查询**
1. 语法
   `SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;`
2. 排序方式
   + ASC: 升序
   + DESC: 降序
```SQL
   [例1]根据年龄对公司员工进行升序排序。
        select * from emp order by age asc;
   [例2]根据年龄对公司员工进行升序排序,年龄相同，再按照入职时间进行降序排序。
        select * from emp order by age asc, entrydate desc;
```
**注意：若多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。**

**六、分页查询**
1. 语法
   `SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;`  
```SQL 
   [例1]查询第1页员工数据，每页显示10条记录。
       select * from emp limit 0,10;--也可以写为limit 10；
   [例2]查询第2页员工数据，每页显示10条记录。
       select * from emp limit 10,10;
```
**注意：**
  + 起始索引从0开始，起始索引=（查询页码-1）*每页显示记录数；
  + 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT。
  + 如果查询的是第一页数据，起始索引可以省略。

**七、DQL执行顺序**
编写顺序：
```SQL
SELECT 字段列表 
FROM 表名列表 
WHERE 条件列表 
GROUP BY 分组字段列表 
HAVING 分组后条件列表 
ORDER BY 排序字段列表 
LIMIT 分页参数
```
执行顺序：
```SQL
FROM 表名列表 
WHERE 条件列表 
GROUP BY 分组字段列表 
HAVING 分组后条件列表 
SELECT 字段列表 
ORDER BY 排序字段列表 
LIMIT 分页参数
```

***
## DCL-数据控制语言
**一、 管理用户**
1. 查询用户
   ```SQL
   USE mysql;
   SELECT * FROM user;
   ```
2. 创建用户
   ```SQL
   CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
   ```
3. 修改用户密码
   ```SQL
   ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
   ```
4. 删除用户
   ```SQL
   DROP USER '用户名'@'主机名';
   ```
**注意：**
+ 主机名可以使用%通配；
+ 这类sql开发人员操作得比较少，主要是DBA(Database Administrator 数据库管理员)使用。

**二、权限控制**
MySQL中定义了很多种权限，而常用的如下表：
| 权限 | 说明 |
| :---: | :---: |
|ALL,ALL PRIVILEGES|所有权限|
|SELECT|查询数据|
|INSERT|插入数据|
|UPDATE|修改数据|
|DELETE|删除数据|
|ALTER|修改表|
|DROP|删除数据库/表/视图|
|CREATE|创建数据库/表|
1. 查询权限
   `SHOW GRANTS FOR '用户名'@'主机名';`
2. 授予权限
   `GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';`
3. 撤销权限
   `REVOKE 权限列表 ON 数据库名。表名 FROM '用户名'@'主机名';`
```SQL
   [例]授予用户heima数据库itcast中所有列表的所欲权限。
       grant all on itcast.* to 'heima'@'%';
```
**注意：**
+ 多个权限之间，用逗号分隔。
+ 授权时，数据库名和表名可以使用*进行通配，代表所有。