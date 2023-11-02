# 2.1. 查询数据

```sql
SELECT * FROM 表名;
SELECT 字段名1, 字段名2 FROM 表名; # 指定列名称查询
SELECT * FROM 表名 WHERE 字段名=字段值; # 根据条件查询
```

## 2.1.1. AS 别名

将返回的字段名设置为别名

```sql
SELECT username AS user, password FROM users;
SELECT *, COUNT(*) AS total FROM users;
```

## 2.1.2. WHERE 子句

**WHERE 子句**用于限定选择的标准。在 **SELECT、UPDATE、DELETE 语句**中，皆可使用。
**运算符：**
[MySQL 运算符 | 菜鸟教程](https://www.runoob.com/mysql/mysql-operator.html)

### 2.1.2.1. NULL

```sql
SELECT Name, ROLE FROM employees WHERE Building IS NULL;
```

在`SQL`中，我们判断字段是否为`NULL`，需要使用`IS NULL`、`IS NOT NULL`，而不能使用`=`来进行判断，因为`字段名 = NULL`返回的是`UNKNOWN`而不是布尔值。

### 2.1.2.2. LIKE 查询

```sql
SELECT * FROM 表名 WHERE 字段名 LIKE '%keyword%'; -- 包含查询
SELECT * FROM 表名 WHERE 字段名 LIKE '%keyword'; -- 后缀查询
SELECT * FROM 表名 WHERE 字段名 LIKE 'keyword%'; -- 前缀查询
```

1. %：表示任意长度的任意字符，可代替0个或多个字符。
2. _：表示任意单个字符，可代替1个字符。
3. []：表示匹配指定范围内的任意单个字符，如[abc]表示匹配a、b或c中的任意一个字符；[a-z]表示匹配a到z中的任意一个字符。
4. [^]：表示匹配除指定字符外的任意单个字符，如[^abc]表示匹配除a、b、c之外的任意一个字符。

## 2.1.3. ORDER BY 子句

用于对查询数据进行排序

```sql
SELECT * FROM users ORDER BY id ASC; # 降序
SELECT * FROM users ORDER BY id DESC; # 升序
SELECT * FROM users ORDER BY password DESC, username ASC; # 多重排序
```

两个字段相加然后排序，可以使用一致的条件排序，但是更方便的是使用别名排序

```sql
SELECT Title, (Domestic_sales + International_sales) / Length_minutes AS sale_value FROM movies
LEFT JOIN Boxoffice ON movies.id = Boxoffice.Movie_id
WHERE Director = 'John Lasseter'
ORDER BY sale_value DESC LIMIT 3;
```

## 2.1.4. DISTINCT 去重

```sql
SELECT DISTINCT column, another_column
FROM mytable
WHERE condition(s);
```

DISTINCT 语法介绍，我们拿之前的 Movies表来说，可能很多电影都是同一年Year发布的，如果你想要按年份排重，一年只能出现一部电影到结果中， 你可以用 DISTINCT 关键字来指定某个或某些属性列唯一返回。

## 2.1.5. JOIN

```sql
SELECT column, another_column, mytable.*, another_table.*
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table ON mytable.id = another_table.matching_id
```

**INNER JOIN** 只会保留**两个表都存在的数据**（还记得之前的交集吗），这看起来意味着一些数据的丢失，在某些场景下会有问题。
真实世界中两个表存在差异很正常，所以我们需要更多的连表方式，也就是本节要介绍的左连接`LEFT JOIN`，右连接`RIGHT JOIN`和全连接`FULL JOIN`。这几个连接方式都会保留不能匹配的行。
**注意：**顾名思义，连表会把两张表连在一起，所以column name不能冲突，如果冲突则需要使用`表名.columnName AS 别名`进行处理。

## 2.1.6. 统计函数

| **Function** | **Description** |
| --- | --- |
| COUNT(*) | 计数：COUNT(*) 统计数据行数。 |
| COUNT(column) | 计数：COUNT(column) 统计column非NULL的行数。 |
| MIN(column) | 最小值：找column最小的一行。 |
| MAX(column) | 最大值：找column最大的一行。 |
| AVG(column) | 平均值：对column所有行取平均值。 |
| SUM(column) | 求和：对column所有行求和。 |

**example：**

```sql
SELECT COUNT(*) FROM users WHERE is_delete=false;
```

## 2.1.7. GROUP BY

**GROUP BY** 数据分组语法可以按某个col_name对数据进行分组，如：GROUP BY Year指对数据按年份分组， 相同年份的分到一个组里。如果把统计函数和GROUP BY结合，那统计结果就是对分组内的数据统计了.
**语法：**

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

**example:**

```sql
SELECT role, AVG(Years_employed) FROM employees GROUP BY role;
```

这里我们首先对 role 进行分组，分组之后平均就是年限（这里的AVG对全表中所有相同的 role 的年限平均，而不是平均组内的年限）。

## 2.1.8. HAVING

在 GROUP BY 分组语法中，我们知道数据库是先对数据做WHERE，然后对结果做分组，如果我们要对分组完的数据再筛选出几条如何办？ （想一下按年份统计电影票房，要筛选出>100万的年份？）
一个不常用的语法 HAVING 语法将用来解决这个问题，他可以对分组之后的数据再做SELECT筛选。

```sql
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

**example:**

```sql
SELECT
SUM(Domestic_sales + International_sales) as sum_sale, -- 总销量
Director, -- Director
COUNT(*) as moves_count, -- 电影数量
(SUM(Domestic_sales + International_sales) / COUNT(*)) AS avg_sale -- 平均销量
FROM movies
JOIN Boxoffice ON movies.ID = Boxoffice.Movie_id
GROUP BY Director
HAVING moves_count > 1
ORDER BY avg_sale DESC LIMIT 1;
```

## 2.1.9. PostgreSQL JSON查询

```sql
-- 查询JSON数据
SELECT info FROM orders;
-- { "customer": "John Doe", "items": {"product": "Beer","qty": 6}}

-- 查找JSON中的key
SELECT info -> 'customer' AS customerKey FROM orders;
-- "customer"

-- 查找JSON中的value
SELECT info -> 'items' ->> 'product' AS productValue FROM orders;
-- Beer
```

按键返回JSON对象字段：`->`，类型是：`json`
按文本返回JSON对象字段：`->>`，类型是：`text`

### 2.1.9.1. 在WHERE子句中使用JSON操作符

```sql
SELECT
 info -> 'items' ->> 'product' AS customer
FROM
 orders
WHERE
 info -> 'items' ->> 'product' = 'Beer'
```

**example：**

```sql
SELECT
 info ->> 'customer' AS customer,
 info -> 'items' ->> 'product' AS procut
FROM
 orders
WHERE
 CAST(info -> 'items' ->> 'qty' AS INTEGER) = 6
```

**注意：**我们使用cast转换qty字段值为integer类型，然后和2进行比较。

### 2.1.9.2. JSON聚集函数

我们能对json数据使用聚集函数，如min,max,average,sum等。

```sql
SELECT
   MIN (
     CAST (
       info -> 'items' ->> 'qty' AS INTEGER
     )
   ),
   MAX (
     CAST (
       info -> 'items' ->> 'qty' AS INTEGER
     )
   ),
   SUM (
     CAST (
       info -> 'items' ->> 'qty' AS INTEGER
     )
   ),
   AVG (
     CAST (
       info -> 'items' ->> 'qty' AS INTEGER
     )
   )
 
FROM
   orders
```

### 2.1.9.3. JSON函数

```sql
-- 将最外层的JSON对象展开为一组键值对
SELECT json_each (info) FROM orders;
-- 得到一组key-value对作为文本
SELECT json_each_text(info) FROM orders;
-- 获得json对象最外层的一组键
SELECT json_object_keys(info -> 'items') AS jsonKey FROM orders;
-- 获得数据的类型
SELECT json_typeof (info->'items'->'qty') FROM orders;
```

## 2.1.10 MySQL JSON查询

### 2.1.10.1. 数组查询

`JSON_CONTAINS(target, candidate[, path])`
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21870146/1669725535200-f5524520-dbca-4260-956b-5ee18b0df6d4.png#averageHue=%23fbfdc7&clientId=u702ee55b-7ff4-4&from=paste&height=58&id=skl5G&originHeight=72&originWidth=469&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3916&status=done&style=none&taskId=u2b1b869f-6f09-46a8-95f2-cb87da3424f&title=&width=375.2)

```sql
SELECT * from user WHERE JSON_CONTAINS(ext, '1')
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21870146/1669723788024-496512b6-9235-4ec7-a9e7-1ab748e18b3a.png#averageHue=%23d2f3d8&clientId=u702ee55b-7ff4-4&from=paste&height=42&id=HUtdO&originHeight=52&originWidth=479&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3347&status=done&style=none&taskId=ufdbedecb-0ea6-4bb5-9418-6162cf5ecec&title=&width=383.2)

```sql
SELECT * from user WHERE JSON_CONTAINS(ext, JSON_OBJECT("a", 1))
```

### 2.1.10.2. 提取 JSON

`JSON_EXTRACT(json_doc, path[, path] ...)`
**提取普通JSON：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21870146/1669724135811-dead34ea-36d4-4af4-8b70-db682697c7f9.png#averageHue=%23fbfdc7&clientId=u702ee55b-7ff4-4&from=paste&height=60&id=qUd7k&originHeight=75&originWidth=476&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4368&status=done&style=none&taskId=u6c81f838-a62d-46f1-9d42-af02de33598&title=&width=380.8)

```sql
SELECT * from user WHERE JSON_EXTRACT(ext, "$.a") = 1
```

**提取数组JSON：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21870146/1669724685822-c07ab568-2bf1-4cde-9025-555126374424.png#averageHue=%23f9fac2&clientId=u702ee55b-7ff4-4&from=paste&height=62&id=PvnFa&originHeight=77&originWidth=469&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4139&status=done&style=none&taskId=u0a5efde0-8341-4f0f-bdb2-7d27eefe9bc&title=&width=375.2)

```sql
SELECT * from user WHERE JSON_EXTRACT(ext, "$[0]") = "aa"
```

## 2.1.11. 子查询

```sql
SELECT 
title,
(SELECT 
  MAX(Domestic_sales + International_sales) 
 FROM Boxoffice) 
- (Domestic_sales + International_sales) as balance
FROM movies
LEFT JOIN Boxoffice ON movies.id = Boxoffice.Movie_id
```

找出每部电影和单部电影销售冠军之间的销售差，列出电影名，销售额差额