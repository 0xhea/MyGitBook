
# 查询(query) SELECT

## 基础查询

```mysql
SELECT 查询列表 FROM 表名;
```

1. 查询列表可以是字段、常量、表达式、函数，也可以是多个
2. 查询结果是一个虚拟表

### 查询表中的字段

```mysql
SELECT 字段名 FROM 表名;
SELECT 字段名，字段名 FROM 表名;  # 查询多个字段
SELECT * FROM 表名;  # 查询所有字段
```

```mysql
SELECT 常量值;  # 常量
SELECT 函数名(实参列表);  # 函数
SELECT 100/1234;  # 表达式
```


注意：字符型和日期型的常量值必须用单引号引起来，数值型不需要。

### 起别名 (AS, 空格)
```mysql
SELECT last_name AS 姓,first_name AS 名 FROM employees;
SELECT last_name 姓,first_name 名 FROM employees;
```

如果要查询的字段有重名的情况，使用别名可以区分开来

### 去重 distinct
`SELECT distinct 字段名 FROM 表名;`

### +号 做加法运算
```mysql
SELECT 数值+数值;  # 直接运算
SELECT 字符+数值;  # 先试图将字符转换成数值，如果转换成功，则继续运算；否则转换成0，再做运算
SELECT null+值;  # 结果都为null
```

### 补充函数

#### concat 函数

功能：拼接字符
`SELECT concat(字符1，字符2，字符3,...);`

#### ifnull 函数
功能：判断某字段或表达式是否为null，如果为null 返回指定的值，否则返回原本的值
`SELECT ifnull(commission_pct,0) FROM employees;`

#### isnull 函数
功能：判断某字段或表达式是否为null，如果是，则返回1，否则返回0.



## 条件查询 WHERE

```mysql
SELECT 查询列表 FROM 表名 WHERE 筛选条件;
```

### 简单条件运算符

<	=	<>	!=	>=	<=	<=>安全等于

### 逻辑运算符

&& and

|| or

! not

### 模糊查询

#### like

```mysql
SELECT * FROM employees WHERE last_name like '%a%';
SELECT last_name FROM employees WHERE last_name LIKE '_\_%';  # 转义
SELECT last_name FROM employees WHERE last_name LIKE '_$_%' ESCAPE '$';  # ESCAPE可以用来指定转义符
```

一般搭配通配符使用，可以判断字符型或数值型

通配符：%任意多个字符，_任意单个字符

#### between and

`SELECT * FROM employees WHERE employee_id BETWEEN 120 AND 100;`

#### in

`SELECT last_name, job_id FROM employees WHERE job_id IN( 'IT_PROT' ,'AD_VP','AD_PRES');`

#### is null / is not null

`SELECT last_name, commission_pct FROM employees WHERE commission_pct IS NOT NULL;`

=或<>不能用于判断null值，is null或is not null 可以判断null值

|              | 普通类型的数值 | null值 | 可读性 |
| ------------ | -------------- | ------ | ------ |
| is null      | ×              | √      | √      |
| <=> 安全等于 | √              | √      | ×      |



## 排序查询 ORDER BY

```mysql 
SELECT 查询列表 FROM 表 [WHERE 筛选条件] ORDER BY 排序列表 [ASC/DESC]
```

1. asc升序，desc降序，默认升序；
2. order by 子句可以支持单个字段、多个字段、表达式、函数、别名；
3. order by 子句一般是放在查询语句的最后面，limit子句除外。

```mysql
SELECT * FROM employees WHERE department_id >= 90 ORDER BY hiredate ASC;

# 按表达式排序
SELECT *, salary * 12 * (1 + IFNULL(commission_pct, 0)) AS 年薪 FROM employees 
ORDER BY salary * 12 * (1 + IFNULL(commission_pct, 0)) ASC;
# 按别名排序
SELECT *, salary * 12 * (1 + IFNULL(commission_pct, 0)) AS 年薪 FROM employees 
ORDER BY 年薪 ASC;

# 按函数排序
SELECT LENGTH(last_name) AS 姓名长度, last_name, salary FROM employees 
ORDER BY LENGTH(last_name) DESC;

# 按多个字段排序
SELECT * FROM employees ORDER BY salary ASC, employee_id DESC;
```



## 常见函数

```mysql
SELECT 函数名(实参列表) [FROM 表];
```

### 单行函数

#### 字符函数

1. length 获取字节长度（utf8一个中文三个字节）
2. concat 拼接字符串
3. upper / lower 字符串全大写/小写
4. substr substring 截取字符串（索引从1开始）
5. instr 返回子串在字符串中第一次出现位置的索引，如果不存在返回0
6. trim 去掉字符串首尾的字符，默认为空格 `SELECT TRIM('a' FROM 'aababaa'); #bab`
7. lpad(str, len, padstr)  在字符串左侧填充到指定长度， 如果len小于str长度，str会被右截断
   * `SELECT LPAD('zhou', 9, 'ab'); #ababazhou`
   * `SELECT LPAD('zhou', 2, 'ab'); #zh`
8. rpad(str, len, padstr) 右填充
9. replace 替换 `SELECT REPLACE('abcab', 'ab', 'de'); #decde`

#### 数学函数

1. round 四舍五入
   * ROUND(X) `SELECT ROUND(1.567); #2`
   * ROUND(X, D) `SELECT ROUND(1.567, 2); #1.57`
2. ceil(x) 向上取整，返回>=该参数的最小整数
3. floor(x) 向下取整，返回<=该参数的最小整数
4. truncate(x, d) 小数点后保留d位，直接截断
5. mod(n, m) 取余数（复数情况等价于 n-n/m*m）
6. rand 获取随机数，返回 0-1 之间的小数

#### 日期函数

1. now 返回当前系统日期和时间
2. curdate 返回当前系统日期
3. curtime 返回当前时间
4. year、month、monthname、day、hour、minute、second
5. str_to_date 将日期格式的字符转换成指定格式的日期
   * `SELECT STR_TO_DATE('1998-3-2', '%Y-%c-%d') AS out_put; #1998-03-02`
6. date_format 将日期转换成字符
   * `SELECT DATE_FORMAT(NOW(), '%y年%m月%d日') AS output; #20年01月29日`
7. DATEDIFF 计算日期相差天数

#### 其他函数

1. version 数据库服务器的版本号
2. database 当前打开的数据库
3. user 当前用户
4. password(str)  返回 str 的密码形式
5. md5(str)  返回 str 的 md5 加密形式

#### 流程控制函数

1. IF(expr1, expr2, expr3) 如果expr1成立返回expr2，否则返回expr3

   * `SELECT IF(10>5, '大', '小');`

2. case

   1. ```mysql
      # 类似switch case
      case 要判断的字段或表达式
      when 常量1 then 要显示的值1或语句1; # 值不需要加; 语句要加;
      when 常量2 then 要显示的值2或语句2;
      ...
      else 要显示的值n或语句n;
      end
      ```

   2. ```mysql
      # 类似多重if
      case
      when 条件1 then 要显示的值1或语句1; # 值不需要加; 语句要加;
      when 条件2 then 要显示的值2或语句2;
      ...
      else 要显示的值n或语句n;
      end
      ```



### 分组函数（统计函数，聚合函数，组函数）

做统计使用

1. sum 求和
2. avg 平均值
3. max 最大值
4. min 最小值
5. count 计算非空值的个数

说明：

1. sum、avg一把用于处理数值型
2. 上面5个分组函数都忽略null值
3. 可以和distinct搭配实现去重 `select sum(distinct 字段) from 表;`
4. count(*) 用于统计行数
5. 和分组函数一同查询的字段要求是group by后的字段



## 分组查询 GROUP BY

```mysql
SELECT column, group_function(column)
FROM table
WHERE condition
GROUP BY group_by_expression
ORDER BY column
HAVING condition;
```

### 分组查询筛选

#### 分组前筛选 where

筛选原始表中的数据，使用 **where**

#### 分组后的筛选 having

筛选分组后的结果集，使用 **having**

1. 分组函数做条件肯定是放在having子句中。
2. 能用分组前筛选的，就优先考虑使用分组前筛选。

```mysql
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 2;
```

### 按表达式或函数分组

```mysql
SELECT department_id, job_id, AVG(salary)
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id, job_id
HAVING AVG(salary) > 10000
ORDER BY AVG(salary);
```



## 连接查询（多表查询）

当查询的字段来自于多个表时，会用到连接查询。

```mysql
# 笛卡尔乘积现象，没有有效的连接条件
SELECT NAME, boyName FROM beauty, boys;

SELECT NAME, boyName FROM beauty, boys
WHERE beauty.boyfriend_id = boys.id;
```

### 分类

#### 按年代分类

##### sql92标准

仅仅支持内连接

##### sql99标准

支持内连接 + 外连接（左外和右外） + 交叉连接

#### 按功能分类

##### 内连接

1. 等值连接
2. 非等值连接
3. 自连接

##### 外连接

1. 左外连接
2. 右外连接
3. 全外链接

##### 交叉连接

### sql92 标准

#### 等值连接

1. 多表等值连接的结果为多表的交集部分
2. n 表连接，至少需要 n-1 个连接条件
3. 多表的顺序没有要求
4. 一般需要为表起别名
5. 可以搭配前面介绍的所有子句使用，比如：排序、分组、筛选...

```mysql
SELECT last_name, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id;
```

##### 为表起别名

提高语句的简洁度，区分多个重名的字段

注意：**如果为表起了别名，则查询的字段就不能使用原来的表名去限定**

```mysql
SELECT last_name, e.job_id, job_title
FROM employees AS e, jobs AS j
WHERE e.job_id = j.`job_id`;
```

##### 各种情况

```mysql
# 加筛选
SELECT department_name, city
FROM departments d, locations l
WHERE d.`location_id` = l.`location_id`
AND l.`city` LIKE '_o%';
# 加分组
SELECT department_name, d.manager_id, MIN(salary)
FROM employees e, departments d
WHERE e.`department_id` = d.`department_id`
AND e.`commission_pct` IS NOT NULL
GROUP BY d.`department_name`, d.`manager_id`;
# 加排序
SELECT j.`job_title`, COUNT(*)
FROM employees e, jobs j
WHERE j.`job_id` = e.`job_id`
GROUP BY j.`job_title`
ORDER BY COUNT(*) DESC;
# 多个表
SELECT last_name, department_name, city
FROM employees e, departments d, locations l
WHERE e.`department_id` = d.`department_id`
AND l.`location_id` = d.`location_id`;
```

#### 非等值连接

```mysql
SELECT salary, grade_level
FROM employees e, job_grades g
WHERE salary BETWEEN g.`lowest_sal` AND g.`highest_sal`
AND g.`grade_level` = 'A';
```

#### 自连接

```mysql
SELECT e.`employee_id`, e.`last_name`, m.`employee_id`, m.`last_name`
FROM employees e, employees m
WHERE e.`manager_id` = m.`employee_id`;
```

### sql99 标准

```mysql
select 查询列表
from 表1 别名
连接类型 join 表2 别名
on 连接条件
[where 筛选条件]
[group by 分组]
[having 筛选条件]
[order by 排序列表]
```

#### 连接类型

内连接：inner

左外连接：left [outer]

右外连接：right [outer]

全外连接（mysql不支持）：full [outer]

交叉连接：cross

#### 内连接

##### 等值连接

1. 可以添加筛选、排序、分组

2. **inner** 可以省略
3. inner join 连接和 sql92 语法中的等值连接效果一样，都是查询多表的交集。

```mysql
SELECT last_name, department_name
FROM employees e
INNER JOIN departments d
ON e.`department_id` = d.`department_id`;

# 添加筛选
SELECT last_name, job_title
FROM employees e
INNER JOIN jobs j
ON e.`job_id` = j.`job_id`
WHERE last_name LIKE '%e%';

# 添加分组+筛选
SELECT city, COUNT(*)
FROM departments d
INNER JOIN locations l
ON d.`location_id` = l.`location_id`
GROUP BY city
HAVING COUNT(*) > 3;

# 排序
SELECT department_name, COUNT(*)
FROM employees e
INNER JOIN departments d
ON e.`department_id` = d.`department_id`
GROUP BY department_name
HAVING COUNT(*) > 3
ORDER BY COUNT(*) DESC;

# 三表连接
SELECT last_name, department_name, job_title
FROM employees e
INNER JOIN departments d ON e.`department_id` = d.`department_id`
INNER JOIN jobs j ON e.`job_id` = j.`job_id`
ORDER BY department_name DESC;
```

##### 非等值连接

```mysql
SELECT last_name, salary, grade_level
FROM employees e
INNER JOIN job_grades g
ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`;

# 排序、筛选、分组
SELECT grade_level, COUNT(*)
FROM employees e
INNER JOIN job_grades g
ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`
GROUP BY grade_level
HAVING COUNT(*) > 20
ORDER BY grade_level DESC;
```

##### 自连接

```mysql
SELECT e.last_name, e.employee_id, m.last_name, m.employee_id
FROM employees e
INNER JOIN employees m
ON e.`manager_id` = m.`employee_id`;
```

#### 外连接

应用场景：用于查询一个表中有，另一个表没有的记录。

1. 外连接的查询结果为主表中的所有记录。

   如果从表中有和它匹配的，则显示匹配的值。如果从表中没有和它匹配的，则显示null。

   **外连接查询结果 = 内连接结果 + 主表中有而从表没有的记录。**

2. 左外连接，left join 左边的是主表

   右外连接，right join 右边的是主表

3. 左外和右外交换两个表的顺序，可以实现同样的效果

```mysql
SELECT g.name
FROM beauty g
LEFT OUTER JOIN boys b
ON g.`boyfriend_id` = b.`id`
WHERE b.`id` IS NULL;

SELECT d.*, e.`employee_id`
FROM departments d
LEFT OUTER JOIN employees e
ON d.`department_id` = e.`department_id`
WHERE e.`employee_id` IS NULL;
```

##### 全外连接（mysql不支持）

全外连接 = 内连接结果 + 表1中有但表2中没有的 + 表2中有但表1中没有的

```MYSQL
SELECT b.*, g.*
FROM beauty g
FULL OUTER JOIN boys b
ON g.`boyfriend_id` = b.id
```

#### 交叉连接

笛卡尔乘积

```mysql
SELECT b.*, bo.*
FROM beauty b
CROSS JOIN boys bo;
```



## 子查询

出现在其他语句中的 select 语句，称为子查询或内查询。

外部如果为select查询语句，则此语句称为主查询或外查询。外部的语句可以是insert、update、delete、select等

### 分类

#### 按子查询出现的位置

##### select 后面：仅仅支持标量子查询

##### from 后面：表子查询

##### where 或 having 后面：标量子查询（单行）※，列子查询（多行）※，行子查询（用处较少）

##### exists 后面（相关子查询）：表子查询

#### 按结果集的行列数不同

##### 标量子查询（结果集只有一行一列）

##### 列子查询（结果集只有一列多行）

##### 行子查询（结果集有一行多列或多行多列）

##### 表子查询（结果集一般为多行多列）

### where 或 having 后面

1. 标量子查询（单行子查询）
2. 列子查询（多行子查询）
3. 行子查询（多列多行）

#### 特点

1. 子查询放在小括号内

2. 子查询一般放在条件的右侧

3. 标量子查询，一般搭配着单行操作符使用 > < >= <= = <>

   列子查询，一般搭配着多行操作符使用 in、any / some、all

4. 子查询的执行优先于主查询执行，主查询的条件用到了子查询的结果

#### 标量子查询（单行子查询）

```mysql
SELECT last_name, job_id, salary
FROM employees
WHERE salary = (
	SELECT MIN(salary)
	FROM employees
);

SELECT department_id, MIN(salary)
FROM employees
GROUP BY department_id
HAVING MIN(salary) > (
	SELECT MIN(salary)
	FROM employees
	WHERE department_id = 50
);
```

#### 列子查询（多行子查询）

1. 返回多行

2. 使用多行比较操作符

| 操作符      | 含义                              |
| ----------- | --------------------------------- |
| IN / NOT IN | 等于列表中的任意一个 / 不在列表中 |
| ANY \| SOME | 和子查询返回的某一个值比较        |
| ALL         | 和子查询返回的所有值比较          |

```mysql
SELECT last_name
FROM employees
WHERE department_id IN (
	SELECT DISTINCT department_id
	FROM departments
	WHERE location_id IN (1400, 1700)
);

SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE job_id <> 'IT_PROG'
AND salary < ANY(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id = 'IT_PROG'
);

SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE job_id <> 'IT_PROG'
AND salary < ALL(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id = 'IT_PROG'
);
```

#### 行子查询（结果集有一行多列或多行多列）

```mysql
SELECT *
FROM employees
WHERE (employee_id, salary) = (
	SELECT MIN(employee_id), MAX(salary)
	FROM employees
);
```

### select 后面

仅仅支持标量子查询

```mysql
SELECT d.*, (
	SELECT COUNT(*)
	FROM employees e
	WHERE e.department_id = d.department_id
)
FROM departments d;
```

### from 后面

将子查询结果充当一张表，要求必须起别名

```mysql
SELECT ag_dep.*, g.grade_level
FROM (
	SELECT department_id, AVG(salary) ag
	FROM employees e
	GROUP BY department_id
) ag_dep
INNER JOIN job_grades g
ON ag_dep.ag BETWEEN lowest_sal AND highest_sal;
```

### exists 后面（相关子查询）

```mysql
exists(完整的查询语句)
```

判断子查询中是否有数据。

如果存在数据返回1，否则返回0

```mysql
SELECT department_name
FROM departments d
WHERE EXISTS(
	SELECT *
	FROM employees e
	WHERE e.`department_id` = d.`department_id`
);
```



## 分页查询 LIMIT ※

当要显示的数据，一页显示不全，需要分页提交sql请求

```mysql
select 查询列表							# 执行顺序 ⑦
from 表										  # ①
[连接类型 join 表2							   # ②
on 连接条件										# ③
where 筛选条件									# ④
group by 分组字段								# ⑤
having 分组后的筛选							   # ⑥
order by 排序的字段]								# ⑧
limit [offset, ]size;						   # ⑨
```

* offset 要显示条目的起始索引（起始索引从0开始）。如果索引是0可省略。
* size 要显示的条目个数

### 特点

1. limit 语句放在查询语句的最后，并且也是最后执行

2. 公式

   ```mysql
   # 要显示的页数page， 每页的条目数size
   select 查询列表
   form 表
   limit (page - 1) * size, size;
   ```

```mysql
SELECT * FROM employees LIMIT 0, 5;
SELECT * FROM employees LIMIT 5;
```



## 联合查询 UNION

将多条查询语句的结果合并成一个结果。

### 应用场景

要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时使用联合查询。

### 注意事项

1. 要求多条查询语句的查询列数一致
2. 要求多条查询语句的查询的每一列的类型和顺序最好一致
3. `UNION` 关键字默认去重，如果使用 `UNION ALL` 可以包含重复项。

```mysql
查询语句1
union
查询语句2
union
...
```

```mysql
SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id > 90;

# union all 不去重
SELECT id, cname FROM t_ca WHERE csex = '男'
UNION ALL
SELECT t_id, tname FROM t_ua WHERE tGender = 'male';
```













