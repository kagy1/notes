# MySQL

## 与oracle的区别

1. 在 MySQL 中，可以直接执行表达式

   ```sql
   SELECT 'hello world';
   ```

   MySQL 的 `dual` 并不是一个真正的表，而是一个特殊的伪表，它只在查询表达式时起作用。

2. 数据类型

   - MySQL 中没有 `VARCHAR2`，用 `VARCHAR` 替代
   - Oracle 中的 `NUMBER` 可用于整数和小数，而在 MySQL 中需要用 `INT` 或 `DECIMAL`。
   - Oracle 的 `DATE` 数据类型包含日期和时间，而 MySQL 中 `DATE` 只包含日期，需要用 `DATETIME` 或 `TIMESTAMP` 处理日期时间。



## 数据库

### 创建数据库

```sql
create database reggieTestL character set utf8mb4;
```

### 选择数据库

```sql
USE reggieTestL;
```

