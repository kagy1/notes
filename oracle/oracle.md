# Oracle

## 注释

```sql
-- 注释内容
```



## 基本语法

### 创建表

```sql
CREATE TABLE test_multiple_cols (
  col1 DATE,
  col2 VARCHAR(50),
  col3 INT
);
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



## 连接

### left join



### (+) 左外连接

`students` 表包含以下数据：

| student_id | student_name |
| :--------- | :----------- |
| 1          | Alice        |
| 2          | Bob          |
| 3          | Charlie      |

`classes` 表包含以下数据：

| class_id | class_name | student_id |
| :------- | :--------- | :--------- |
| 1        | Math       | 1          |
| 2        | English    | 2          |

现在，我们想要查询所有学生的信息以及他们所选修的课程（如果有的话）。我们可以使用左外连接来实现：

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

查询结果将是：

| student_id | student_name | class_name |
| :--------- | :----------- | :--------- |
| 1          | Alice        | Math       |
| 2          | Bob          | English    |
| 3          | Charlie      | (null)     |





## 函数

### Mod

标量数值函数，返回一个数除以另一个数的模数（余数）

```sql
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

