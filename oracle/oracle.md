# Oracle

## 注释

```sql
-- 注释内容
```



## 基本语法

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



### 修改表

1. 主键约束

```sql
添加
alter table product add constraint PK_PRODUCT_PID primary key(pid);
删除
alter table product drop constraint PK_PRODUCT_PID;
或者
alter table product drop primary key;
```

2. 非空约束

```sql
添加
alter table product modify pname not null;
删除
alter table product modify pname null;
```



### 选择语句

#### in

```sql
select * from Customers where state in('VA','GA','Fl')
-- 等同于
select * from Customers where state = 'VA' or 'GA' or 'FL'
```



### trunc

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



## 用户

### 用户类型

normal，sysdba，sysoper



### 用户信息表

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

### 用户权限表

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

当然,以下是 Oracle 中一些常用的字符型函数:

1. LENGTH(char) - 返回字符串的长度。
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
15. CHR(n) - 返回 ASCII 码为 n 的字符。
16. ASCII(char) - 返回字符的 ASCII 码。
17. LTRIM(char [, set]) 和 RTRIM(char [, set]) - 从字符串的开头或结尾删除 set 中指定的字符。如果省略 set,则删除空格。



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



## 事务

### 含义

一条或多条sql语句组成一个执行单位，一组sql语句要么都执行要么都不执行

### 特点

1. 原子性：一个事务是不可再分割的整体，要么都执行要么都不执行
2. 一致性：一个事务的执行不能破坏数据库数据的完整性和一致性
3. 隔离性：一个事务不受其它事务的干扰，多个事务是互相隔离的
4. 持久性：一个事务一旦提交了，则永久的持久化到本地











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

