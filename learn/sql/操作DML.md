# 操作

## 插入 INSERT

### 方式一：经典插入

```mysql
INSERT INTO 表名(列名1, ...) VALUES(值1, ...);
```

1. 插入的值的类型要与列的类型一致或兼容
2. 可以为 null 的列如何插入值？
   1. 值填null
   2. 不写对应列名
3. 列的顺序可以调换
4. 列数和值的个数必须一致、对应
5. 可以省略列名，默认是所有列，而且列的顺序和表中列的顺序一致

```mysql
INSERT INTO beauty(id, NAME, sex, borndate, phone, photo, boyfriend_id)
VALUES(13, '唐艺昕', '女', '1990-4-23', '1898888888', NULL, 2);
```

### 方式二

```mysql
INSERT INTO 表名
SET 列名=值, 列名=值, ...
```

```mysql
INSERT INTO beauty
SET id = 19, NAME = '刘涛', phone = '999';
```

| 方式一（用的较多） | 方式二       |
| ------------------ | ------------ |
| 可以插入多行       | 只能插入一行 |
| 支持子查询         | 不支持子查询 |

```mysql
# 插入多行
INSERT INTO beauty(id, NAME, sex, borndate, phone, boyfriend_id)
VALUES(14, '唐艺昕1', '女', '1990-4-23', '1898888888', 2),
(15, '唐艺昕2', '女', '1990-4-23', '1898888888', 2),
(16, '唐艺昕3', '女', '1990-4-23', '1898888888', 2);

# 支持子查询（插入查询语句的结果集）
INSERT INTO beauty(id, NAME, phone)
SELECT 26, '宋茜', '11809866';
```



## 修改 UPDATE

### 修改单表的记录 ※

```mysql
UPDATE 表名
SET 列=新值, 列=新值, ...
WHERE 筛选条件;
```

```mysql
UPDATE beauty
SET phone = '1388888888'
WHERE NAME LIKE '唐%';

UPDATE boys
SET boyName = '张飞', userCP = 10
WHERE id = 2;
```

### 修改多表的记录

```mysql
# sql92 语法
UPDATE 表1 别名, 表2 别名
SET 列=新值, 列=新值, ...
WHERE 连接条件
AND 筛选条件;

# sql99 语法
UPDATE 表1 别名
[连接类型(inner|left|right)] JOIN 表2 别名
ON 连接条件
SET 列=新值, 列=新值, ...
WHERE 筛选条件
```

```mysql
UPDATE boys bo
INNER JOIN beauty b
ON b.boyfriend_id = bo.id
SET phone = '114'
WHERE bo.`boyName` = '张无忌';

UPDATE beauty b
LEFT OUTER JOIN boys bo
ON b.`boyfriend_id` = bo.`id`
SET b.`boyfriend_id` = 2
WHERE bo.`id` IS NULL;
```



## 删除 DELETE / TRUNCATE

### 方式一：DELETE

#### 单表的删除 ※

```mysql
DELETE FROM 表名
WHERE 筛选条件;
```

```mysql
DELETE FROM beauty
WHERE phone LIKE '%9'
```

#### 多表的删除（级联删除）

```mysql
# sql92 语法
delete [表1的别名], [表2的别名] # 删除哪个表的数据写哪个别名
from 表1 别名, 表2 别名
where 连接条件
and 筛选条件;

# sql99 语法
delete [表1的别名], [表2的别名] # 删除哪个表的数据写哪个别名
from 表1 别名
[连接类型(inner|left|right)] JOIN 表2 别名
on 连接条件
where 筛选条件;
```

```mysql
DELETE b
FROM beauty b
INNER JOIN boys bo
ON b.boyfriend_id = bo.id
WHERE bo.boyName = '张无忌';

DELETE b, bo
FROM beauty b
INNER JOIN boys bo
ON b.boyfriend_id = bo.id
WHERE bo.boyName = '黄晓明';
```

### 方式二：TRUNCATE

删除表中所有数据。

```mysql
# truncate 语句不允许加 where
TRUNCATE TABLE 表名;
```

| 对比 ※                         | delete             | truncate       |
| ------------------------------ | ------------------ | -------------- |
| 是否可以加 WHERE 筛选          | 可以加 WHERE 条件  | 不允许加 WHERE |
| 效率                           | 效率比 truncate 低 | 效率较高       |
| 删除后再插入数据，自增长列的值 | 从断点开始         | 从1开始        |
| 返回值（是否显示n行受影响）    | 有返回值           | 没有返回值     |
| 回滚                           | 可以回滚           | 不能回滚       |

