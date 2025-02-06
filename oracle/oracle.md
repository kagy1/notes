# Oracle

## 注释

```sql
-- 注释内容
```



## DDL

### 创建表

```sql
CREATE TABLE 表名(
        列名 列的类型 primary key,--主键约束
        列名 列的类型 not null,--非空约束
        列名 列的类型 unique,--唯一约束
    	列名 列的类型 check(列名 in (检查列表)),--检查约束
    	列名 列的类型 deafult 默认值 约束类型,
        constraint 约束名 foreign key(字段名) references 主表(被引用列)--外键约束
);

COMMENT ON COLUMN 表名.列名 IS '注释内容';
```

```sql
--商品分类表
create table category(
       cid number primary key,
       cname varchar2(20),
       CYDY_FLAG CHAR default '0' not null,
       production_date DATE default SYSDATE, -- 生产日期，默认值为当前时间
);

--商品详情表
create table product(
       pid number primary key,--主键约束
       pname varchar2(50) not null,--非空约束
       pimg varchar2(50) unique,--唯一约束
       pflag varchar2(10) check(pflag in ('上架','下架')),--检查约束
       cid number,
       constraint FK_CATEGORY_ID foreign key(cid) references category(cid)--外键约束
);
```



### 列的类型

1. 字符类型：
   - CHAR(n)：<span style="color:red">固定长度的字符串</span>，最多可存储 2000 个字节。<span style="color:red">如果存储的字符串长度不足 n，会用空格填充。</span>
   - VARCHAR2(n)：可变长度的字符串，最多可存储 4000 个字节。存储的字符串长度可以小于 n。
   - NCHAR(n)：固定长度的 Unicode 字符串，最多可存储 2000 个字节。
   - NVARCHAR2(n)：可变长度的 Unicode 字符串，最多可存储 4000 个字节。
   - LONG：可变长度的字符串，最多可存储 2GB。不建议在新的应用程序中使用，建议使用 CLOB 类型代替。
   - 目前oracle并未实现varchar，与varchar2完全一致
2. 数值类型：
   - NUMBER(p, s)：可变长度的数值类型，p 表示精度（总位数），s 表示小数点后的位数（范围是 -84 到 127）。
   - FLOAT(p)：浮点数，p 表示精度（二进制位数），范围是 1 到 126。
   - BINARY_FLOAT：32 位单精度浮点数。
   - BINARY_DOUBLE：64 位双精度浮点数。
3. 日期和时间类型：
   - DATE：存储日期和时间信息，精确到秒。
   - TIMESTAMP(n)：存储日期和时间信息，精确到秒的小数部分，n 表示小数秒的精度（范围是 0 到 9）。
   - TIMESTAMP(n) WITH TIME ZONE：存储日期、时间和时区信息，精确到秒的小数部分。
   - TIMESTAMP(n) WITH LOCAL TIME ZONE：存储日期和时间信息，精确到秒的小数部分，并根据会话的时区进行调整。
   - INTERVAL YEAR(p) TO MONTH：存储年和月的时间间隔，p 表示年的精度。
   - INTERVAL DAY(p) TO SECOND(s)：存储天、小时、分钟和秒的时间间隔，p 表示天的精度，s 表示秒的精度。
4. 大对象类型：
   - BLOB：存储大型二进制对象，最多可存储 4GB。
   - CLOB：存储大型字符数据，最多可存储 4GB。
   - NCLOB：存储大型 Unicode 字符数据，最多可存储 4GB。
   - BFILE：存储外部文件的二进制数据，最多可存储 4GB。
5. RAW 类型：
   - RAW(n)：存储二进制或字节数据，最多可存储 2000 个字节。
   - LONG RAW：存储可变长度的二进制或字节数据，最多可存储 2GB。不建议在新的应用程序中使用，建议使用 BLOB 类型代替。
6. 其他类型：
   - ROWID：存储行的唯一标识符，用于快速访问数据库中的特定行。
   - UROWID：存储行的唯一标识符，用于在分布式环境中唯一标识一行数据。
   - XMLType：存储和管理 XML 数据。



### 复制表

表会复制源表的数据,但不会完全复制源表的所有属性。具体来说:

1. 列的数据类型:新表中的列将具有与源表中对应列相同的数据类型。
2. 列的名称:新表中的列名将与 SELECT 语句中指定的列名相同。如果使用 `*` 选择所有列,则新表的列名将与源表的列名相同。
3. 数据:新表将包含 SELECT 语句返回的所有数据。

但是,以下属性不会被复制到新表:

1. 约束:源表的主键、外键、唯一键和检查约束不会被复制到新表。
2. 索引:源表上的索引不会被复制到新表。
3. 触发器:附加到源表的触发器不会被复制到新表。
4. 列的默认值:源表列的默认值不会被复制到新表。新表中的所有列都将有默认值 NULL。
5. 列的 NOT NULL 约束:源表列的 NOT NULL 约束不会被复制到新表。新表中的所有列都将允许 NULL 值。
6. 列的注释:源表列的注释不会被复制到新表。

```sql
create table 表名 as 查询语句;

create table emp_copy as 
     select * from emp
;
```



### 删除表

```sql
-- 方式一：
drop table 表名;
-- 方式二：
truncate table 表名;
```

- DROP TABLE 会完全删除表的结构和数据，并且会从数据字典中移除表的定义。执行后，该表不再存在。
- TRUNCATE TABLE 只删除表中的所有数据，但保留表的结构。执行后，该表仍然存在，只是变成了一个空表。



### 修改表

添加列

add

```sql
-- 格式：
alter table 表名 add 列名 列的类型;
-- 演示：
alter table users add phone varchar2(11);
```

修改列名

rename

```sql
-- 格式：
alter table 表名 rename column 旧列名 to 新列名;
-- 演示：
alter table users rename column phone to mobile;
```

修改类型

modify

```sql
-- 格式：
alter table 表名 modify 列名 列的类型;
-- 演示：
alter table users modify mobile char(11);
```

删除列

drop

```sql
-- 格式：
alter table 表名 drop column 列名;
-- 演示：
alter table users drop column mobile;
```





## DML

### 插入

```sql
-- 格式：
insert into 表名(列名1,列名2,...) values(值1,值2,...);
-- 演示：
INSERT INTO course(cid, cname, credit) VALUES(1, '数学', 3);
INSERT INTO course(cid, cname, credit) VALUES(2, '英语', 2);
INSERT INTO course(cid, cname, credit) VALUES(3, '计算机基础', 4);
```

### 修改

```sql
-- 格式：
update 表名 set 列名1=值1,列名2=值2,... where 查询条件;
-- 演示：
update category set cname='汽车' where cid = 1;
```

### 删除

```sql
-- 格式：
delete from 表名 where 查询条件;
-- 演示：
delete from category where cid = 1;
```







## 选择语句

### in

```sql
select * from Customers where state in('VA','GA','Fl')
-- 等同于
select * from Customers where state = 'VA' or 'GA' or 'FL'
```





## trunc

对值进行截断，通常用于截断数字和日期

```sql
trunc(suorce, length)
-- source 表示被截断的数字，length 表示保留小数位数，或者是日期的格式项 yyyy,mm,dd hh,hh24,mi,ss 。
```

- 截断数字

```sql
select trunc(3.14159) from dual;  -- 3
select trunc(3.14159,0) from dual;  -- 3
select trunc(3.14159,1) from dual;  -- 3.1
select trunc(3.14159,3) from dual;  -- 3.141
select trunc(314159.67767,-3) from dual;  -- 截取整数 314000
```

- 截断日期

```sql
select sysdate,trunc(sysdate,'yyyy') from dual;-- 截取到年：2021/9/5 16:48:05  2021/1/1
select sysdate,trunc(sysdate,'mm') from dual;-- 截取到月：2021/9/5 16:48:05  2021/9/1
select sysdate,trunc(sysdate,'dd') from dual;-- 截取到天：2021/9/5 16:48:05   2021/9/5
select sysdate,trunc(sysdate,'hh24') from dual;-- 截取到时：2021/9/5 16:48:05   2021/9/5 16:00:00
select sysdate,trunc(sysdate,'mi') from dual;-- 截取到分：2021/9/5 16:48:05   2021/9/5 16:48:00
select sysdate,trunc(add_months(sysdate,-1),'mm') from dual;-- 获取上月第一天 2021/9/5 16:48:05   2021/8/1
select sysdate,trunc(sysdate) from dual;-- 获取今天的日期：2021/9/5 16:48:05   2021/9/5
select sysdate,trunc(sysdate,'d') from dual;-- 获取当前星期的第一天(星期天) 2021/9/5 16:48:05  2021/9/5
```



## SQL语言的分类

1. DQL：数据查询语言：select、from、where
2. DCL：数据控制语言：grant、revoke
3. DDL：数据定义语言：create、alter、drop、truncate
4. DML：数据操作语言：insert、update、delete
5. TCL：事务控制语言：commit、rollback



## 连接

### 外连接

#### 左外连接

```sql
SELECT s.student_id, s.student_name, c.class_name
FROM students s, classes c
WHERE s.student_id = c.student_id(+);
```

等同于:

```sql
SELECT s.student_id, s.student_name, c.class_name
FROM students s
LEFT JOIN classes c ON s.student_id = c.student_id;
```





### 交叉连接

隐式交叉连接

```sql
select * from emp, dept;
```

显示交叉连接

```sql
select * from emp e1 cross join dept d1;
```



## 函数

### 聚合函数

**count**

```sql
SELECT COUNT(*) FROM employees; -- 统计employees表的总行数
SELECT COUNT(DISTINCT department_id) FROM employees; -- 统计employees表中不同的department_id数量
```

**sum**

```sql
SELECT SUM(salary) FROM employees; -- 计算所有员工的工资总和
SELECT SUM(DISTINCT salary) FROM employees; -- 计算所有不同工资值的总和
```

**avg**

```sql
SELECT AVG(salary) FROM employees; -- 计算所有员工的平均工资
SELECT AVG(DISTINCT salary) FROM employees; -- 计算所有不同工资值的平均值
```

**max**

```sql
SELECT MAX(salary) FROM employees; -- 查询员工表中的最高工资
```

**min**

```sql
SELECT MIN(salary) FROM employees; -- 查询员工表中的最低工资
SELECT MIN(hire_date) FROM employees; -- 查询员工表中最早的入职日期
```

**LISTAGG**

将组内的多个字符串连接成一个字符串。Oracle 11g中引入

语法: LISTAGG(column [, delimiter]) WITHIN GROUP (ORDER BY column)

```sql
SELECT department_id, LISTAGG(last_name, ', ') WITHIN GROUP (ORDER BY last_name) AS employees
FROM employees
GROUP BY department_id;  -- 按部门分组,把每个部门的员工姓名用逗号连接成一个字符串
```



### 数值函数

1. ABS(n) - 返回 n 的绝对值。

2. CEIL(n) 和 CEILING(n) - 向上取整，返回大于或等于 n 的最小整数。

3. FLOOR(n) - 向下取整，返回小于或等于 n 的最大整数。

4. ROUND(n [,m]) - 返回 n 四舍五入后保留 m 位小数的值。如果省略 m,则返回最接近的整数。

   ```sql
   select round(3.1415) from dual;  -- 3
   select round(3.1415,2) from dual;  -- 3.14
   ```

5. TRUNC(n [,m]) - 返回 n 截断为 m 位小数的值。如果省略 m,则截断到整数。

   ```sql
   SELECT TRUNC(1234.5678) FROM DUAL;  -- 1234
   SELECT TRUNC(1234.5678, 2) FROM DUAL;  -- 1234.56
   SELECT TRUNC(1234.5678, -1) FROM DUAL;  -- 1230
   SELECT TRUNC(TO_DATE('2023-04-18 15:30:45', 'YYYY-MM-DD HH24:MI:SS')) FROM DUAL;  -- 2023-04-18
   UPDATE employees SET salary = TRUNC(salary, -2);
   ```

6. MOD(n1, n2) - 返回 n1 除以 n2 的余数。

7. POWER(n1, n2) - 返回 n1 的 n2 次幂。

8. SQRT(n) - 返回 n 的平方根。

9. SIGN(n) - 当 n 为正数时返回 1,为负数时返回 -1,为零时返回 0。



### 字符型函数

1. LENGTH(char) - 返回字符串的长度
2. SUBSTR(char, m [, n]) - 返回字符串的子串,从位置 m 开始,截取 n 个字符。如果省略 n,则返回从位置 m 到字符串末尾的子串。
3. INSTR(char, substr [, m [, n]]) - 在字符串 char 中查找子串 substr,返回子串的位置。可选参数 m 指定开始搜索的位置,n 指定第 n 次出现的位置。
4. CONCAT(char1, char2) - 连接两个字符串。
5. UPPER(char) - 将字符串转换为大写。
6. LOWER(char) - 将字符串转换为小写。
7. INITCAP(char) - 将字符串中每个单词的首字母转换为大写,其余字母转换为小写。
8. LPAD(char, n [, padstr]) 和 RPAD(char, n [, padstr]) - 在字符串的左侧或右侧填充字符,使字符串的长度达到 n。如果省略 padstr,则使用空格填充。
9. TRIM([leading | trailing | both] [trimstr] from char) - 从字符串的开头、结尾或两端删除指定的字符。如果省略 trimstr,则删除空格。
10. REPLACE(char, search_string, replacement_string) - 将字符串中的 search_string 替换为 replacement_string。
11. REGEXP_LIKE(char, pattern [, match_option]) - 检查字符串是否匹配正则表达式 pattern。
12. REGEXP_SUBSTR(char, pattern [, position [, occurrence [, match_param]]]) - 返回字符串中匹配正则表达式 pattern 的子串。
13. REGEXP_REPLACE(char, pattern [, replacement [, position [, occurrence [, match_param]]]]) - 将字符串中匹配正则表达式 pattern 的部分替换为 replacement。
14. TRANSLATE(char, from_string, to_string) - 将字符串中的字符依次替换为 to_string 中对应位置的字符。
15. LTRIM(char [, set]) 和 RTRIM(char [, set]) - 从字符串的开头或结尾删除 set 中指定的字符。如果省略 set,则删除空格。



### 分析函数

1. ROW_NUMBER() OVER (ORDER BY ...) - 为结果集中的每一行分配一个唯一的连续数字。

2. RANK() OVER (ORDER BY ...) - 为结果集中的每一行分配一个排名,排名可能不连续。

3. DENSE_RANK() OVER (ORDER BY ...) - 为结果集中的每一行分配一个排名,排名是连续的。

4. LAG(expr [, offset] [, default]) OVER (ORDER BY ...) - 返回当前行的前 offset 行的 expr 值。如果前 offset 行不存在,则返回 default。

5. LEAD(expr [, offset] [, default]) OVER (ORDER BY ...) - 返回当前行的后 offset 行的 expr 值。如果后 offset 行不存在,则返回 default。

6. FIRST_VALUE(expr) OVER (ORDER BY ... [ROWS|RANGE BETWEEN ...]) - 返回窗口框架中第一行的 expr 值。

7. LAST_VALUE(expr) OVER (ORDER BY ... [ROWS|RANGE BETWEEN ...]) - 返回窗口框架中最后一行的 expr 值。

8. SUM(expr) OVER (ORDER BY ... [ROWS|RANGE BETWEEN ...]) - 计算窗口框架中 expr 的累计和。

   输出的行数与原始数据的行数相同

9. AVG(expr) OVER (ORDER BY ... [ROWS|RANGE BETWEEN ...]) - 计算窗口框架中 expr 的累计平均值。

10. COUNT(expr) OVER (ORDER BY ... [ROWS|RANGE BETWEEN ...]) - 计算窗口框架中 expr 的累计计数。



### 其他函数

1. NVL(expr1, expr2) - 如果 expr1 为 NULL,则返回 expr2;否则返回 expr1。

2. COALESCE(expr1, expr2, ..., exprn) - 返回第一个非 NULL 的表达式。

3. DECODE(expr, search1, result1 [, search2, result2 ] ... [, default]) - 将 expr 依次与 search 比较,返回匹配的 result。如果没有匹配,则返回 default。

   ```sql
   -- 根据 department_id 返回不同的部门名称
   SELECT employee_id, DECODE(department_id, 
                              10, 'Administration',
                              20, 'Marketing',
                              30, 'Purchasing',
                              40, 'Human Resources',
                              50, 'Shipping',
                              'Other') AS department
   FROM employees;
   ```

4. CASE WHEN condition1 THEN result1 [WHEN condition2 THEN result2] ... [ELSE default] END - 根据条件返回不同的结果。

   ```sql
   -- 根据 salary 返回不同的工资等级
   SELECT employee_id, salary,
          CASE 
            WHEN salary >= 15000 THEN 'A'
            WHEN salary >= 10000 THEN 'B'
            WHEN salary >= 5000 THEN 'C'
            ELSE 'D'
          END AS salary_grade
   FROM employees;
   ```

5. GREATEST(expr1, expr2, ..., exprn) - 返回表达式列表中的最大值。

   ```sql
   -- GREATEST 和 LEAST
   SELECT GREATEST(10, 20, 30) FROM DUAL; -- 返回 30
   ```

6. LEAST(expr1, expr2, ..., exprn) - 返回表达式列表中的最小值。

   ```sql
   SELECT LEAST('Apple', 'Banana', 'Cherry') FROM DUAL; -- 返回 'Apple'
   SELECT LEAST(SYSDATE, DATE '2023-12-31') FROM DUAL; -- 返回当前日期
   ```

7. NULLIF(expr1, expr2) - 如果 expr1 等于 expr2,则返回 NULL;否则返回 expr1。

8. TO_CHAR(date|number [, format]) - 将日期或数字转换为字符串。

9. TO_DATE(char [, format]) - 将字符串转换为日期。

10. TO_NUMBER(char [, format]) - 将字符串转换为数字。

11. CAST(expr AS type) - 将表达式转换为指定的数据类型。



## 分组查询

```sql
--统计每个部门有多少个人
select deptno as "部门",count(*) as "人数" from emp group by deptno;

--统计部门人数>5人的部门的编号
select deptno as "部门", count(*) as "人数"
from emp
group by deptno
having count(*) > 5;
```



## 查询排序

```sql
--按照员工主管编号由高到低进行排序，NULL值放到最后边
select * from emp order by mgr desc nulls last;

-- 按照员工主管编号由高到低进行排序，NULL值放到最前边
select * from emp order by mgr desc nulls first;

-- CASE 表达式排序，根据条件确定 NULL 值的位置
SELECT * FROM emp
ORDER BY
    CASE WHEN mgr IS NULL THEN 1 ELSE 0 END ASC,
    mgr ASC;
```



## 分页查询

### 简单分页

伪列rownum ， `rownum` 的值是在 `where` 子句执行之后才生成的。这意味着在 `where` 子句中使用 `rownum` 来限制结果集的时候,会先根据 `rownum` 的条件选出数据,然后才生成 `rownum` 的值。

```sql
select rownum,t.* from T_ACCOUNT t where rownum <= 10
```

跳过前 10 行数据,可以使用子查询

```sql
select * 
from (
  select rownum as rn, t.*
  from T_ACCOUNT t
)
where rn > 10;
```



## 联合查询

1. 列的类型要一致
2. 列的顺序要一致
3. 列的数量要一致，如果不够，可以使用null填充



### 并集运算

```sql
/*
	union 		: 它会去除重复的，并且排序
	union all 	: 不会去除重复的，不会排序
*/

--工资大于1500或者20号部门下的员工
select * from emp where sal > 1500
union
select * from emp where deptno = 20;

--工资大于1500或者20号部门下的员工
select * from emp where sal > 1500
union all
select * from emp where deptno = 20;
```



### 交集运算

找两个查询结果的交集

```sql
--工资大于1500并且20号部门下的员工
select * from emp where sal > 1500
intersect
select * from emp where deptno = 20;
```



### 差集运算

找两个查询结果的差集

```sql
--1981年入职员工(不包括总裁和经理)
select * from emp where to_char(hiredate,'yyyy') = '1981'
minus
select * from emp where job = 'PRESIDENT' or job = 'MANAGER';
```



## 子查询

- 单行子查询：>、>=、<、<=、!=、<>、=、<=>
- 多行子查询：in、not in、any(some)、all、exits

```sql
--查询所有经理的信息
select * from emp where empno in (select mgr from emp where mgr is not null);

select * from his.PAT_INFO where PAT_ID not in (select pid from adt.inpatient);

--查询出比10号部门任意一个员工薪资高的员工信息
select * from emp where sal > any (select sal from emp where deptno = 10);
```

exits

```sql
-- 查找门诊病人
select * from his.pat_info i 
where not exists(
    select 1
    from adt.INPATIENT a
    where a.pid = i.PAT_ID
);
```



## 日期 & 时间

### 基本类型

Oracle支持不同的日期格式模型，其中包括：

1. ISO 8601: `YYYY-MM-DDTHH:MI:SS`，例如2024-06-20T14:30:00
2. Oracle内部格式: `DD-MON-YYYY HH:MI:SS AM`，例如20-JUN-2024 02:30:00 PM



#### DATE

存储日期和时间，精确到秒

```sql
CREATE TABLE test_date (col DATE);
-- 创建一个名为 test_date 的表,该表包含一个类型为 DATE 的列 col
INSERT INTO test_date (col) VALUES (TO_DATE('2024-06-20 12:34:56', 'YYYY-MM-DD HH24:MI:SS'));
```

#### TIMESTAMP

比DATE类型更精确，可以精确到小数秒

```sql
CREATE TABLE test_timestamp (col TIMESTAMP);
INSERT INTO test_timestamp (col) VALUES (TO_TIMESTAMP('2024-06-20 12:34:56.789', 'YYYY-MM-DD HH24:MI:SS.FF3'));
```

#### INTERVAL YEAR TO MONTH

存储年份和月份的时间间隔

```sql
CREATE TABLE test_interval_ym (col INTERVAL YEAR TO MONTH);
INSERT INTO test_interval_ym (col) VALUES (INTERVAL '2-3' YEAR TO MONTH);
```

#### INTERVAL DAY TO SECOND

存储天、小时、分钟、秒以及小数秒的时间间隔

```sql
CREATE TABLE test_interval_ds (col INTERVAL DAY TO SECOND);
INSERT INTO test_interval_ds (col) VALUES (INTERVAL '5 12:34:56.789' DAY TO SECOND);
```



### 常用函数

#### SYSDATE

返回当前系统日期和时间

```sql
SELECT SYSDATE FROM dual;
```

#### current_date

返回当前会话时区的日期

#### CURRENT_TIMESTAMP

返回当前系统时间戳

```sql
SELECT CURRENT_TIMESTAMP FROM dual;
```

#### EXTRACT

从日期或时间戳中提取特定的部分（如年、月、日、小时等）

```sql
SELECT EXTRACT(YEAR FROM SYSDATE) AS year FROM dual; # 2024
SELECT EXTRACT(MONTH FROM SYSDATE) AS month FROM dual; # 6
SELECT EXTRACT(DAY FROM SYSDATE) AS day FROM dual; # 20
```

#### TO_DATE

将字符串转换为DATE类型

```sql
SELECT TO_DATE('2024-06-20', 'YYYY-MM-DD') FROM dual;
select TO_DATE('2022-01-01 18:34:56', 'YYYY-MM-DD HH24:MI:SS')from dual;
```

#### TO_TIMESTAMP

将字符串转换为TIMESTAMP类型

```sql
SELECT TO_TIMESTAMP('2024-06-20 12:34:56.789', 'YYYY-MM-DDHH24:MI:SS.FF3') FROM dual;
```

#### TO_CHAR

将日期或时间戳转换为字符串，可以指定格式

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') FROM dual;  -- 2024-08-08 14:04:02
select to_char(sysdate , 'yyyy"年"MM"月"dd"日"')  from  dual;  -- 2024年08月08日
select to_char(sysdate , 'yyyy"年"MM"月"dd"日" HH24"时"MI"分"SS"秒"') from dual; -- 2024年08月08日 14时05分29秒
```

#### ADD_MONTHS

给日期加上指定的月份数

```sql
SELECT ADD_MONTHS(SYSDATE, 6) AS new_date FROM dual;
```

#### MONTHS_BETWEEN

计算两个日期之间的月份数

```sql
SELECT MONTHS_BETWEEN(TO_DATE('2024-12-20', 'YYYY-MM-DD'), SYSDATE) AS months_between FROM dual;
```

#### NEXT_DAY

返回指定日期之后的第一个指定星期几

```sql
SELECT NEXT_DAY(SYSDATE, 'FRIDAY') AS next_friday FROM dual;
```

#### LAST_DAY

返回指定月份的最后一天

```sql
SELECT LAST_DAY(SYSDATE) AS last_day_of_month FROM dual;
```



### 时区

#### 查询时区

```sql
SELECT SESSIONTIMEZONE FROM DUAL;
```

#### 设置时区

```sql
ALTER SESSION SET TIME_ZONE = '+08:00'; -- 设置为中国标准时间(北京时间)
```



## DCL

### 表空间

#### 创建表空间

```sql
create tablespace 表空间的名称
datafile '文件的路径'
size 初始化大小
autoextend on
next 每次扩展的大小;
```

示例

```sql
create tablespace mytest
datafile 'd:/mytest.dbf'
size 100m
autoextend on
next 10m;
```

#### 删除表空间

```sql
drop tablespace 表空间的名称;
-- 示例
drop tablespace mytest;
```

#### 查询表空间

`DBA_TABLESPACES` 视图、`USER_TABLESPACES` 视图

```sql
SELECT * FROM dba_data_files;
SELECT * FROM user_tablespaces;
```



### 用户

#### 创建用户

```sql
create user 用户名
identified by 密码
default tablespace 表空间的名称;
```

演示

```sql
create user kagy
identified by "123456"
default tablespace HIS;
```



#### 用户类型

normal，sysdba，sysoper



#### 用户信息表

dba_users保存了所有用户信息

all_users保存了所有用户信息。该表的属性信息比dba_users少

user_users表保存当前用户信息

```sql
-- 查询所有用户
SELECT * FROM DBA_USERS;
SELECT * FROM all_users;

-- 查询当前用户
SELECT user FROM dual;
-- 查询当前用户信息
select * from USER_USERS;
```

#### 用户权限表

```sql
-- 查询当前用户的所有权限
SELECT * FROM session_privs;

-- 查看当前用户的系统权限
select * from user_sys_privs;

-- 查询指定用户的系统权限
SELECT * FROM dba_sys_privs WHERE grantee = 'username';

-- 查询指定用户的对象权限
SELECT * FROM dba_tab_privs WHERE grantee = 'username';

-- 查询指定用户的角色
SELECT * FROM dba_role_privs WHERE grantee = 'username';

```



#### 授权用户

```sql
grant 系统权限列表 to 用户名;
-- 或者
grant 实体权限列表 on 表名称 to 用户名;
```

示例

```sql
grant CONNECT to zhangsan;
-- 或者
grant CONNECT,RESOURCE to zhangsan;
-- 或者
grant CONNECT,RESOURCE,DBA to zhangsan;
-- 或者
grant DBA to zhangsan;
-- 或者
grant all on emp to zhangsan;

-- 赋予所有权限
GRANT ALL PRIVILEGES TO kagy;
```



#### 权限列表

系统权限分类：（系统权限只能由DBA用户授出）

- DBA：拥有全部特权，是系统最高权限，只有DBA才可以创建数据库结构。
- RESOURCE：拥有Resource权限的用户只可以创建实体，不可以创建数据库结构。
- CONNECT：拥有Connect权限的用户只可以登录Oracle，不可以创建实体，不可以创建数据库结构。

实体权限分类：select、update、insert、alter、index、delete、all



#### 取消授权

系统权限只能由DBA用户回收

```sql
revoke 系统权限列表 from 用户名;
-- 或者
revoke 实体权限列表 on 表名称 from 用户名;
```

示例

```sql
revoke CONNECT from zhangsan;
或者
revoke CONNECT,RESOURCE from zhangsan;
或者
revoke CONNECT,RESOURCE,DBA from zhangsan;
或者
revoke DBA from zhangsan;
或者
revoke all on emp from zhangsan;

-- 取消所有权限
revoke all PRIVILEGES FROM kagy;
```



#### 修改密码

```sql
alter user 用户名 identified by "密码";
alter user zhangsan identified by "123456";
```



## 事务 TCL

### 含义

一条或多条sql语句组成一个执行单位，一组sql语句要么都执行要么都不执行

### 特点 ACID

1. 原子性(**Atomicity**)：一个事务是不可再分割的整体，要么都执行要么都不执行
2. 一致性(**Consistency**)：一个事务的执行不能破坏数据库数据的完整性和一致性
3. 隔离性(**Isolation**)：一个事务不受其它事务的干扰，多个事务是互相隔离的
4. 持久性(**Durability**)：一个事务一旦提交了，则永久的持久化到本地



### 使用

Oracle 11g中事务是隐式自动开始的，它不需要用户显示的执行开始事务语句

```sql
-- 【设置回滚点】
savepoint 回滚点名;

-- 结束事务
-- 提交：
commit;
-- 回滚：
rollback;
-- 回滚到指定的地方： 
rollback to 回滚点名;
```



```sql
INSERT INTO employees(employee_id, last_name, hire_date, salary)
VALUES(1, 'Smith', SYSDATE, 5000);

SAVEPOINT save1;

UPDATE employees SET salary = salary * 1.1 WHERE employee_id = 1;

ROLLBACK TO SAVEPOINT save1;
```

在这个例子中,我们在第一条INSERT语句后设置了保存点`save1`,然后执行了一条UPDATE语句。如果我们想撤销UPDATE操作,但保留INSERT操作,可以使用`ROLLBACK TO SAVEPOINT save1`命令,这将回滚到`save1`点,撤销UPDATE操作,但保留INSERT操作。











## PLSQL

在任何一个ORACLE 的PL/SQL块中至少需要一个BEGIN..END来表示这是一个完整的块。这些PL/SQL块包括DECLARE开头的自定义虚拟块,存储过程,函数,包等

### 格式

```sql
declare
  --声明变量
          
begin
  --业务逻辑

end;
```



### 变量

```sql
declare
  --声明变量
  -- 格式一：变量名 变量类型;
  -- 格式二：变量名 变量类型 := 初始值;
  -- 格式三：变量名 变量类型 := &文本框名;
  -- 格式四：变量名 表名.字段名%type;
  -- 格式五：变量名 表名%rowtype;
  vnum number;
  vage number := 28;
  vabc number := &abc;--输入一个数值，从一个文本框输入
  vsal emp.sal%type;  --引用型的变量，代表emp.sal的类型
  vrow emp%rowtype;   --记录型的变量，代表emp一行的类型          
begin
  --业务逻辑
  dbms_output.put_line(vnum);                       --输出一个未赋值的变量
  dbms_output.put_line(vage);                       --输出一个已赋值的变量
  dbms_output.put_line(vabc);                       --输出一个文本框输入的变量
  select sal into vsal from emp where empno = 7654; --将查询到的sal内容存入vsal并输出
  dbms_output.put_line(vsal);
  select * into vrow from emp where empno = 7654;   --将查询到的一行内容存入vrow并输出  
  dbms_output.put_line(vrow.sal);
  dbms_output.put_line(123);                        --输出一个整数
  dbms_output.put_line(123.456);                    --输出一个小数  
  dbms_output.put_line('Hello,World');              --输出一个字符串
  dbms_output.put_line('Hello'||',World');          --输出一个拼接的字符串，||拼接符Oracle特有
  dbms_output.put_line(concat('Hello',',World'));   --输出一个拼接的字符串，concat函数比较通用
end;

```





### while循环

```sql
while 条件 loop
        
end loop;

--输出1~10
declare
  i number := 1;
begin
  while i <= 10 loop
    dbms_output.put_line(i);
    i := i + 1;
  end loop;
end;
```



### for循环

```sql
for 变量  in [reverse] 起始值..结束值 loop
        
end loop;

--输出1~10
declare

begin
  for i in  1 .. 10 loop
    dbms_output.put_line(i);
  end loop;
end;

-- 输出1加到10
declare
   num1 number := 0;
begin
   for i in 1..10 loop
        num1 := num1 + i;
   end loop;
    DBMS_OUTPUT.put_line(num1);
end;
```



### loop循环

```sql
loop
  
  exit when 条件
  
end loop;

--输出1~10
declare
  i number := 1;
begin
  loop
    exit when i > 10;
    dbms_output.put_line(i);
    i := i + 1;
  end loop;
end;
```



### 意外

```sql
declare
   --声明变量
begin
   --业务逻辑
exception
   --处理异常
   when 异常1 then
     ...
   when 异常2 then
     ...
   when others then
     ...处理其它异常
end;
```

分类

系统异常

- zero_divide ：除数为零异常
- value_error ：类型转换异常
- no_data_found : 没有找到数据
- too_many_rows : 查询出多行记录，但是赋值给了%rowtype一行数据变量

自定义异常

```sql
declare
   --声明变量
   异常名称 exception;
begin
   --业务逻辑
   if 触发条件 then
      raise 异常名称; --抛出自定义的异常
exception
   --处理异常
   when 异常名称 then
     dbms_output.put_line('输出了自定义异常');  
   when others then
     dbms_output.put_line('输出了其它的异常');  
end;
```

```sql
  vi   number;
  vrow emp%rowtype;
begin
  --以下四行对应四个异常，测试请依次放开
  vi := 8/0;  
  --vi := 'aaa';
  --select * into vrow from emp where empno = 1234567;
  --select * into vrow from emp;
exception
  when zero_divide then
    dbms_output.put_line('发生除数为零异常');
  when value_error then
    dbms_output.put_line('发生类型转换异常');
  when no_data_found then
    dbms_output.put_line('没有找到数据异常');
  when too_many_rows then
    dbms_output.put_line('查询出多行记录，但是赋值给了%rowtype一行数据变量');
  when others then
    dbms_output.put_line('发生了其它的异常' || sqlerrm);
end;
```



### if

```sql
IF condition THEN
  statements;
ELSIF condition THEN
  statements;
ELSE
  statements;
END IF;
```

```sql
DECLARE
  v_number NUMBER := 10;
BEGIN
  IF v_number > 0 THEN
    DBMS_OUTPUT.PUT_LINE('Positive number');
  ELSIF v_number < 0 THEN
    DBMS_OUTPUT.PUT_LINE('Negative number');
  ELSE
    DBMS_OUTPUT.PUT_LINE('Zero');
  END IF;
END;
```



### select  into

将查询结果直接赋值给变量

```sql
SELECT column1, column2, ... INTO variable1, variable2, ...
FROM table_name
WHERE condition;
```





## 存储过程

```sql
CREATE [OR REPLACE] PROCEDURE procedure_name
	-- 输入参数（IN）、输出参数（OUT）还是既是输入又是输出参数（IN OUT）。如果省略,默认为 IN。
	-- [DEFAULT default_value]：参数的默认值,如果调用存储过程时没有传入该参数的值,就使用默认值。
    [(parameter_name [IN | OUT | IN OUT] data_type [DEFAULT default_value])]
IS
    -- 声明部分
    -- 在此声明局部变量、游标等
BEGIN
    -- 执行部分
    -- 在此编写存储过程的主体逻辑
    -- 可以包含 SQL 语句和 PL/SQL 控制结构
    
    -- 异常处理部分（可选）
    EXCEPTION
        WHEN exception_name THEN
            -- 处理特定异常的代码
        WHEN OTHERS THEN
            -- 处理其他异常的代码
END procedure_name;
```



### 示例

```sql
create or replace procedure p_ssjk_receive
  (emrid          varchar2, --住院患者的唯一标识
   orderno        varchar2, --商品订单号
   type           varchar2, --销售状态 标识（1发药未结算，2 已退药,3已结算)
   resultCode out varchar2,
   resultMessage out varchar2) 
is
begin
  declare  l_emrid          varchar2(10);
           l_orderno        varchar2(50);
           l_type           varchar2(1);
           l_count          int;
begin
  resultCode:='0';
  resultMessage:='';
  l_emrid:=emrid;
  l_orderno:=orderno;
  l_type:=type;

  if nvl(trim(l_emrid),'0')='0' then
    begin
    resultCode:='-99';
    resultMessage:='入参emrid(住院患者的唯一标识)不能为空!';
    return;
    end; end if;

  if nvl(trim(l_orderno),'0')='0' then
    begin
    resultCode:='-99';
    resultMessage:='入参orderno(商品订单号)不能为空!';
    return;
    end; end if;

  if nvl(trim(l_type),'0')='0' then
    begin
    resultCode:='-99';
    resultMessage:='入参type(销售状态标识)不能为空!';
    return;
    end; end if;

  if nvl(trim(l_type),'0')='1' then      --新增膳食订单信息
    begin
    -- 将查询结果（记录数）赋值给变量 l_count
    select count(*) into l_count from his.meals_orders where orderno=l_orderno;
    if l_count>0 then
      begin
      resultCode:='-1';
      resultMessage:='新增订单失败！商品订单号 '||l_orderno||' 已经存在!';
      return;
      end; end if;

    insert into his.meals_orders (emrid,orderno,type) values (l_emrid,l_orderno,l_type);
    commit;
    resultCode:='1';
    resultMessage:='新增订单成功！';
    return;
    end; end if;

  if nvl(trim(l_type),'0')='2' then      --退膳食订单  修改销售状态 标识（1发药未结算，2 已退药,3已结算)
    begin
    select count(*) into l_count from his.meals_orders where orderno=l_orderno;
    if l_count=0 then
      begin
      resultCode:='-2';
      resultMessage:='退订失败！商品订单号 '||l_orderno||' 不存在!';
      return;
      end; end if;

    select count(*) into l_count from his.meals_orders where type='2' and orderno=l_orderno;
    if l_count>0 then
      begin
      resultCode:='-2';
      resultMessage:='退订失败！商品订单号 '||l_orderno||' 已退订!';
      return;
      end; end if;

    select count(*) into l_count from his.meals_orders where type='3' and orderno=l_orderno;
    if l_count>0 then
      begin
      resultCode:='-2';
      resultMessage:='退订失败！商品订单号 '||l_orderno||' 已结算!';
      return;
      end; end if;

    update his.meals_orders set type='2' where emrid=l_emrid and orderno=l_orderno;
    commit;
    resultCode:='2';
    resultMessage:='商品退订成功！';
    return;
    end; end if;

  if nvl(trim(l_type),'0')='3' then      --结算膳食订单  修改销售状态 标识（1发药未结算，2 已退药,3已结算)
    begin
    select count(*) into l_count from his.meals_orders where orderno=l_orderno;
    if l_count=0 then
      begin
      resultCode:='-3';
      resultMessage:='结算失败！商品订单号 '||l_orderno||' 不存在!';
      return;
      end; end if;

    select count(*) into l_count from his.meals_orders where type='2' and orderno=l_orderno;
    if l_count>0 then
      begin
      resultCode:='-3';
      resultMessage:='结算失败！商品订单号 '||l_orderno||' 已结算!';
      return;
      end; end if;

    select count(*) into l_count from his.meals_orders where type='3' and orderno=l_orderno;
    if l_count>0 then
      begin
      resultCode:='-3';
      resultMessage:='结算失败！商品订单号 '||l_orderno||' 已结算!';
      return;
      end; end if;

    update his.meals_orders set type='3' where emrid=l_emrid and orderno=l_orderno;
    commit;
    resultCode:='3';
    resultMessage:='商品结算成功！';
    return;
    end; end if;

 commit;
end;
end p_ssjk_receive;
```

### 调用

```sql
declare
  v_emrid          varchar2(10) := '1000000001';  -- 住院患者的唯一标识
  v_orderno        varchar2(50) := 'ORDER20230601001';  -- 商品订单号
  v_type           varchar2(1) := '1';  -- 销售状态标识（1发药未结算，2 已退药,3已结算)
  v_resultCode     varchar2(10);
  v_resultMessage  varchar2(200);
begin
  -- 调用存储过程
  p_ssjk_receive(emrid => v_emrid,
                 orderno => v_orderno,
                 type => v_type,
                 resultCode => v_resultCode,
                 resultMessage => v_resultMessage);
                 
  -- 或者
  p_ssjk_receive(v_emrid, v_orderno, v_type, v_resultCode, v_resultMessage);
                 
  -- 输出返回结果
  dbms_output.put_line('resultCode: ' || v_resultCode);
  dbms_output.put_line('resultMessage: ' || v_resultMessage);
exception
  when others then
    dbms_output.put_line('Error occurred: ' || SQLCODE || ' - ' || SQLERRM);
end;
```



### 检查

如果状态为 `INVALID`，需要进一步检查存储过程的创建是否存在问题。

```sql
SELECT OBJECT_NAME, STATUS
FROM USER_OBJECTS
WHERE OBJECT_TYPE = 'PROCEDURE'
  AND OBJECT_NAME = 'CALCULATE_TEST';
```





## 视图

创建视图

```sql
create view 视图名称
as 查询语句
[with read only];


create view view_emp as
select ename,job,mgr from emp;
```

修改视图

```sql
create or replace view 视图名称
as 查询语句
[with read only];

create or replace view view_emp as
select ename,job,mgr,deptno from emp;
```

删除视图

```sql
drop view 视图名称;

drop view view_emp;
```



## 同义词

### 创建同义词

```sql
create [public] synonym 同义词名称 for 对象的名称;

--创建
create synonym syno_emp for emp;
--调用
select * from syno_emp;
```

### 修改同义词

```sql
create or replace [public] synonym 同义词名称 for 对象的名称;

--创建
create or replace synonym syno_emp_update for emp;
--调用
select * from syno_emp_update;
```

### 删除同义词

```sql
drop [public] synonym 同义词名称;

drop synonym syno_emp_update;
```



## 游标

### 语法

```sql
--第一步：定义游标
    --第一种：普通游标
    cursor 游标名[(参数 参数类型)] is 查询语句;
    --第二种：系统引用游标
    游标名 sys_refcursor;

--第二步：打开游标
    --第一种：普通游标
    open 游标名[(参数 参数类型)];
    --第二种：系统引用游标
    open 游标名 for 查询语句;

--第三步：获取一行
	fetch 游标名 into 变量;

--第四步：关闭游标
	close 游标名;
```

### 示例

```sql
DECLARE
  CURSOR c_order_cost IS
    SELECT SFXM_MC, DYZYYZJL_ID, ZYLSH
    FROM his.PAT_INPAT_ORDER_COST
    WHERE PAT_ID ='77004495'
      AND trunc(SQ_RQ) = to_date('2024-10-06','yyyy-mm-dd');

  v_sfxm_mc VARCHAR2(200);
  v_dyzyyzjl_id VARCHAR2(50);
  v_zylsh VARCHAR2(50);
BEGIN
  OPEN c_order_cost;  -- 打开游标

  LOOP
    FETCH c_order_cost INTO v_sfxm_mc, v_dyzyyzjl_id, v_zylsh;

    EXIT WHEN c_order_cost%NOTFOUND;

    DBMS_OUTPUT.PUT_LINE('收费项目:'||v_sfxm_mc||', 医嘱ID:'||v_dyzyyzjl_id||', 住院流水号:'||v_zylsh);
  END LOOP;

  CLOSE c_order_cost;
END;
```

### for循环使用游标

```sql
declare
    cursor v1 is select * from his.PAT_INPAT_ORDER_COST where PAT_ID ='77004495' and trunc(SQ_RQ)= to_date('2024-10-06','yyyy-mm-dd');
begin
    for v in v1 loop
        dbms_output.put_line(v.SFXM_MC);
    end loop;
end;
```







## 索引

### 创建索引

```sql
create [UNIQUE]|[BITMAP] index 索引名 on 表名(列名1,列名2,...);

create index INX_CATEGORY_CNAME on category(cname);

CREATE INDEX IDX_EMPLOYEES_LAST_NAME ON EMPLOYEES (LAST_NAME)  -- 在 EMPLOYEES 表的 LAST_NAME 列上创建索引
  TABLESPACE USERS  -- 指定索引将被创建在 USERS 表空间中
  PCTFREE 10  -- 指定了索引块中保留 10% 的空闲空间，用于未来的索引增长。
  INITRANS 2
  MAXTRANS 255
  STORAGE (
    INITIAL 64K  -- 指定了索引的初始大小为 64 KB
    NEXT 1M  -- 指定了索引的下一个扩展大小为 1 MB
    MINEXTENTS 1  -- 指定了索引的最小扩展数为 1
    MAXEXTENTS UNLIMITED  -- 指定了索引的最大扩展数为无限制
    PCTINCREASE 0 
    BUFFER_POOL DEFAULT
  );
```



### 删除索引

```sql
drop index INX_CATEGORY_CNAME;
```





## for update & for update nowait

### for update

- `FOR UPDATE`子句用于锁定`SELECT`语句检索到的行，以便于进行更新操作。
- 当使用`FOR UPDATE`时，如果所选行已经被其他事务锁定，当前事务将会等待，直到其他事务释放锁定。
- 这样可以防止其他事务同时修改这些行，从而保证数据的一致性。

基本语法

```sql
SELECT column_name
FROM table_name
WHERE condition
FOR UPDATE;
```

### for update nowait

- `FOR UPDATE NOWAIT`的作用和`FOR UPDATE`类似，也是用来锁定`SELECT`语句检索到的行。
- 不同之处在于，<span style="color:red">如果所选行已经被另一个事务锁定，`FOR UPDATE NOWAIT`会立即引发一个错误（通常是一个`ORA-00054`错误），而不是等待其他事务释放锁定。</span>
- 这可以避免数据库事务在等待锁释放时长时间挂起。

基本语法

```sql
SELECT column_name
FROM table_name
WHERE condition
FOR UPDATE NOWAIT;
```

### 使用场景

### 使用场景

- 使用`FOR UPDATE`适合那些可以等待其他事务释放锁定的场景，这样可以逐一处理数据行，确保数据的一致性。
- `FOR UPDATE NOWAIT`适合那些需要立即知道是否可以锁定所需行的场景，如果无法立即获得锁定，则可以迅速做出响应或调整逻辑。

选择使用`FOR UPDATE`还是`FOR UPDATE NOWAIT`取决于具体应用场景和对事务等待时间的容忍度。



## 行列转换（列透视）

### CASE WHEN





## merge into

有则更新，无则插入

```sql
MERGE INTO target_table target_alias
USING source_table source_alias
ON (join_condition)
WHEN MATCHED THEN
    UPDATE SET target_column1 = value1,
               target_column2 = value2, ...
WHEN NOT MATCHED THEN
    INSERT (column1, column2, ...)
    VALUES (value1, value2, ...);
```

```sql
MERGE INTO employees target
USING new_employees source
ON (target.EMP_ID = source.EMP_ID)
WHEN MATCHED THEN
    UPDATE SET target.NAME = source.NAME,
               target.DEPT = source.DEPT
WHEN NOT MATCHED THEN
    INSERT (EMP_ID, NAME, DEPT)
    VALUES (source.EMP_ID, source.NAME, source.DEPT);
```

代码示例

这个情况只有更新没有插入

```sql
<update id="airConditionFeeSwitch">
    begin
    merge into dictmanage.dict_bed_depart t1
    using (select t2.ward_code, t2.fy_id, t2.fb_id
    from dictmanage.dict_ward t2) t
    on (t.ward_code = t1.bq_id)
    when matched then
    update set t1.fy_id = t.fy_id, t1.fb_id = t.fb_id;
    update dictmanage.dict_bed_depart set ktkf_bz=#{dto.ktkf},gnkf_bz=#{dto.gnkf} where fy_id=#{dto.FY_ID} and
    fb_id=#{dto.FB_ID};
    update dictmanage.dict_bed_depart_fee set kf_bz = #{dto.gnkf}
    where fylx = '2' and cwjl_id in (select cwjl_id from dictmanage.dict_bed_depart where fy_id = #{dto.FY_ID} and
    fb_id = #{dto.FB_ID});
    update dictmanage.dict_bed_depart_fee set kf_bz = #{dto.ktkf}
    where fylx = '1' and cwjl_id in (select cwjl_id from dictmanage.dict_bed_depart where fy_id = #{dto.FY_ID} and
    fb_id = #{dto.FB_ID});
    end;
</update>
```

