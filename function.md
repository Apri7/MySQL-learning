# 函数
***
## 一、字符串函数
MySQL内置了很多字符串函数，常用的如下：
| 函数 | 功能 |
| :---: | :---: |
|CONCAT(S1,S2,...,Sn)|字符串拼接|
|LOWER(sttr)|将字符串str全部转为小写|
|UPPER(str)|将字符串str全部转为大写|
|LPAD(str,n,pad)|左填充，在str的左边填充字符串pad直到整个字符串长度为n|
|RPAD(str,n,pad)|右填充，在str的右边填充字符串pad直到整个字符串长度为n|
|TRIM(str)|去掉字符串头部和尾部的空格|
|SUBSTRING(str,start,len)|返回字符串str从start位置起的len个长度字符串）|  

**注意：** substring中字符串的索引从1开始,中间的空格不算。

```SQL
[例]将企业员工的工号统一写为五位数，前面用0填充。
   update emp set workno = lpad(workno,5,'0;);
```

## 二、数值函数
常见的数值函数如下：
| 函数 | 功能 |
| :---: | :---: |
|CEIL(x)|向上取整|
|FLOOR(x)|向下取整|
|MOD(x,y)|返回x%y|
|RAND()|返回0-1内的随机数|
|ROUND(x,y)|求参数x的四舍五入的值，保留y位小数|
```SQL
[例]生成一个六位数的随机验证码。
   select lpad(round(rand()*1000000,0),6,'0');
```

## 三、日期函数
常见的日期函数如下：
| 函数 | 功能 |
| :---: | :---: |
|CURDATE()|返回当前日期|
|CURTIME()|返回当前时间|
|NOW()|返回当前日期和时间|
|YEAR(date)|获取指定date的年份|
|MONTH(date)|获取指定date的月份|
|DAY(date)|获取指定date的日期|
|DATE_ADD(date,INTERVAL expr type)|返回一个日期/时间值加上上一个时间间隔expr后的时间值|
|DATEDIFF(date1,date2)|返回起始时间date1和结束时间date2之间的天数|

```SQL
[例]查询所有员工入职天数，并根据入职天数倒序排序。
   select name, datediff(curdate(), entrydate) as 'entrydays' 
   from emp 
   order by entrydays desc;
   --别名'entrydays'方便书写
```

## 四、流程函数
流程函数也是很常用的一类函数，可以在SQL语句中实现条件筛选，从而提高语句的效率。
| 函数 | 功能 |
| :---: | :---: |
|IF(value, t, f)|如果value为true，则返回t，否则返回f|
|IFNULL(value1, value2)|如果value1不为空，返回value1，否则返回value2|
|CASE WHEN [val1] THEN [res1] ... ELSE [default] END|如果val1为true，返回res1，...，否则返回default默认值|
|CASE [expr] WHEN [val1] THEN [res1] ... ELSE [default] END|如果expr的值等于val1，返回res1，...，否则返回default默认值|

```SQL
[例]统计班级各个学员的成绩，大于等于85分为优秀，大于等于60分为及格，否则为不及格。
   select 
   id, 
   name, 
   (case when math >= 85 then '优秀'when math >= 60 then '及格' else '不及格' end) '数学', 
   (case when english >= 85 then '优秀'when english >= 60 then '及格' else '不及格' end) '英语', 
   (case when chinese >= 85 then '优秀'when chinese >= 60 then '及格' else '不及格' end) '语文'
   from score；
```

