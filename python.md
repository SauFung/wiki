# Python Cheatsheet

## 文件操作


### 数据库
- sqlite

#### SQL 语法
```sql
-- 新建数据库
CREATE DATABASE database_name
-- 新建数据库中的表
CREATE TABLE table_name
(
	col_1, data_type,
    col_2, data_type,
    col_3, data_type,
    col_3, data_type,
	...
)
-- 插入新行
INSERT INTO table_name (col_1, col_2, ...) VALUES (val_1, val_2, ...)
-- 修改表中的数据
-- SET 修改所在列的值
-- WHERE 搜索指定值所在列
UPDATE table_name SET col_name_1 = new_value WHERE col_name_2 = val
-- 删除行
DELETE FROM table_name WHERE col_name = val
-- 删除表
DROP TABLE table_name
-- 清楚表中数据不删除表
TRUNCATE TABLE table_name
-- 删除数据库
DROP DATABASE database_name
-- 选取某个表
SELECT * FORM table_name
-- 选取某个表，指定列
SELECT col_1, col_2, ... FROM table_name
-- 选取某个表，指定行范围
-- LIMIT 指定显示行数，显示多少行
-- OFFSET 可选，指定显示行号，序号从 0 开始（必须使用 LIMIT，不能单独使用）
SELECT * FROM table_name LIMIT num. OFFSET num.
```
> 为方便查看，在 sqlite 命令行中使用 `.mode column` 和 `.header on`
> sqlite 命令行打印表：`SELECT * FORM table_name;`
> 命令不区分大小写


#### sqlite 模块常用 API

- `sqlite3.connect(datebase)`：连接一个数据库
- `connect.cursor()`：创建一个游标
- `cursor.execute(sql, para)`：一个参数执行 SQL 命令，参数可省略
- `cursor.executemany(sql, seq_of_para)`：多个参数执行同一条 SQL 命令
- `cursor.fetchone()`：返回一行数据
- `cursor.fetchall()`：使用列表返回所有行数据
- `cursor.fetchmany(num.)`：使用列表返回多行数据
- `cursor.close()`：关闭游标
- `connect.commit()`：提交操作
- `connect.close()`：关闭连接数据库

使用实例：
```python

```
