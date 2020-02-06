# 定义(define)

## 库的管理

### 创建 CREATE

```mysql
CREATE DATABASE [IF NOT EXISTS] 库名;
```

### 修改 ALTER

#### 重命名（已删除）

```mysql
RENAME DATABASE 旧库名 TO 新库名; # 此条命令已删除
```

#### 修改字符集

```mysql
ALTER DATABASE 库名 CHARACTER SET gbk
```

### 删除 drop

```mysql
DROP DATABASE [IF EXISTS] 库名;
```



## 表的管理

### 创建

```mysql
create table 表名(
	列名 列的类型[(长度) 约束],
	列名 列的类型[(长度) 约束],
	...
	列名 列的类型[(长度) 约束]
)
```

```MYSQL
CREATE TABLE book(
	id INT,
	bName VARCHAR(20),
	price DOUBLE,
	authorId INT,
	publishDate DATETIME
);
```

### 修改

```mysql
alter table 表名 add|drop|modify|change column 列名 [列类型 约束]; 
```

#### 修改列名

```mysql
alter table 表名 change [column] 旧列名 新列名 类型;
```

#### 修改列的类型或约束

```mysql
ALTER TABLE book MODIFY COLUMN pubdate TIMESTAMP;
```

#### 添加新列

```mysql
ALTER TABLE author ADD COLUMN annual DOUBLE;
```

#### 删除列

```mysql
ALTER TABLE author DROP COLUMN annual;
```

#### 修改表名

```mysql
ALTER TABLE author RENAME TO book_author;
```

### 删除

```mysql
drop table [if exists] 表名;
```

### 复制

#### 仅仅复制表的结构

```mysql
CREATE TABLE copy LIKE author;

# 复制某些字段
CREATE TABLE copy4
SELECT id, au_name
FROM author
WHERE 0;
```

#### 复制表的结构+数据

```mysql
CREATE TABLE copy2
SELECT * FROM author;

# 筛选
CREATE TABLE copy3
SELECT id, au_name
FROM author
WHERE nation='中国';
```



## 数据类型

1. 数值型
   1. 整形
   2. 小数
      1. 定点数
      2. 浮点数
2. 字符型
   1. 较短的文本：char、varchar
   2. 较长的文本：text、blob（较长的二进制数据）
3. 日期型

选择原则：所选择的类型越简单越好，能保存数值的类型越小越好

### 整形

| 整数类型     | 字节 | 范围                                      |
| ------------ | ---- | ----------------------------------------- |
| tinyint      | 1    | 有符号：-128~127<br />无符号：0~255       |
| smallint     | 2    | 有符号：-32768~32767<br />无符号：0~65535 |
| mediumint    | 3    | 2^(8 * 3)                                 |
| int、integer | 4    | 2^(8 * 4)                                 |
| bigint       | 8    | 2^(8 * 8)                                 |

1. 默认是有符号，可以通过添加 unsigned 关键字设置无符号
2. 如果插入的数值超过范围，会报出警告，并且插入的值为临界值
3. 如果不设置长度，会有默认的长度。长度是指显示结果的最小长度，需要搭配 ZEROFILL 关键字，不足长度左边补0。（**ZEROFILL 关键字会将数据变为无符号**）

```mysql
CREATE TABLE tab_int(
	t1 INT(7) ZEROFILL,
	t2 INT(7) UNSIGNED
);
```

### 小数

| 浮点数类型   | 字节 | 范围 |
| ------------ | ---- | ---- |
| float(M, D)  | 4    |      |
| double(M, D) | 8    |      |

| 定点数类型                | 字节  | 范围 |
| ------------------------- | ----- | ---- |
| DEC(M, D) / DECIMAL(M, D) | M + 2 |      |

1. M和D代表各部分长度，如果超出范围，则插入临界值

   M：整数部分长度 + 小数部分长度

   D：小数部分长度

2. M和D都可以省略

   如果是 decimal，则M默认为10，D默认为0

   如果是 float 和 double， 则会根据插入的数值的精度来决定精度

3. 定点型的精确度较高

```mysql
CREATE TABLE tab_float(
	f1 FLOAT(5, 2)
	f2 DOUBLE(5, 2)
	f3 DECIMAL(5, 2)
);
```

### 字符型

#### 较短的文本

| 字符串类型 | 最多字符数                               | 特点           | 效率 |
| ---------- | ---------------------------------------- | -------------- | ---- |
| char(M)    | M可以省略，默认为1（M为0~255之间的整数） | 固定长度的字符 | 较高 |
| varchar(M) | M不可以省略（M为0~65535之间的整数）      | 可变长度的字符 | 较低 |

#### 较长的文本



text

blob