- [1.1. 常见字段类型](#11-常见字段类型)
  - [1.1.1. 数值类型](#111-数值类型)
  - [1.1.2. 字符类型](#112-字符类型)
    - [1.1.3. 日期/时间类型](#113-日期时间类型)
    - [1.1.4. 布尔类型](#114-布尔类型)
    - [1.1.5. JSON 类型](#115-json-类型)
  - [1.2. 建表](#12-建表)
  - [1.3. 多对多](#13-多对多)

# 1.1. 常见字段类型

## 1.1.1. 数值类型

| **数据类型**     | **别名**  | **说明**                      | **范围**                                     |
| ---------------- | --------- | ----------------------------- | -------------------------------------------- |
| smallint         | int2      | 小范围整数                    | -32768 到 +32767                             |
| integer          | int、int4 | 典型的整数                    | -2147483648 到 +2147483647                   |
| bigint           | int8      | 大范围整数                    | -9223372036854775808 到 +9223372036854775807 |
| decimal(p,s)     |           | 可选精度的精确数字            | 小数点前 131072 位；小数点后 16383 位        |
| numeric(p,s)     |           | 可选精度的精确数字            | 小数点前 131072 位；小数点后 16383 位        |
| real             | float4    | 可变精度，不准确              | 6 位十进制数字精度                           |
| double precision | float8    | 双精度浮点数（8字节），不准确 | 15 位十进制数字精度                          |
| smallserial      | serial2   | 自动增加的小整数              | 1 到 32767                                   |
| serial           | serial4   | 自动增加的整数                | 1 到 2147483647                              |
| bigserial        | serial8   | 自增的大整数                  | 1 到 9223372036854775807                     |

## 1.1.2. 字符类型

| **数据类型**         | **别名**   | **说明**               |
| -------------------- | ---------- | ---------------------- |
| character(n)         | char(n)    | 定长字符串，不足补空格 |
| character varying(n) | varchar(n) | 变长字符串             |
| text                 |            | 无限变长               |

### 1.1.3. 日期/时间类型

| **数据类型**                   | **别名**     | **说明**             |
| ------------------------------ | ------------ | -------------------- |
| date                           |              | 日期，没有时间       |
| timestamp(n) without time zone | timestamp(n) | 日期和时间（无时区） |
| timestamp(n) with time zone    | timestamptz  | 日期和时间，包括时区 |
| time(n) without time zone      | time(n)      | 时间（无时区）       |
| time(n) with time zone         | timetz       | 时间，包括时区       |
| interval fields(n)             | interval(n)  | 时间跨度             |

**注意**：`CURRENT_TIMESTAMP`或`now()`可以作为默认值更新当前时间。

### 1.1.4. 布尔类型

| **数据类型** | **别名** | **说明**            |
| ------------ | -------- | ------------------- |
| boolean      | bool     | 逻辑布尔值（真/假） |

允许关键字`TRUE`、`FALSE`、`NULL`

### 1.1.5. JSON 类型

| **数据类型** | **说明**                 |
| ------------ | ------------------------ |
| json         | 文本 JSON 数据           |
| jsonb        | 二进制 JSON 数据，可分解 |

## 1.2. 建表

```sql
CREATE TABLE 表名(
  id serial NOT NULL PRIMARY KEY,
  name varchar(255)
  ...
);
```

## 1.3. 多对多

1.多对多需要三张表，如图所示
![](https://cdn.nlark.com/yuque/0/2023/png/21870146/1692152783996-9e586f11-04af-4408-9e4d-ccfa6381d6a2.png#averageHue=%23f4f3f1&clientId=uaf182df6-791c-4&from=paste&id=ubf392a8a&originHeight=183&originWidth=342&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=ucb73c652-8265-48a3-a673-59f8f272103&title=)![](https://cdn.nlark.com/yuque/0/2023/png/21870146/1692152793775-574e4eea-a778-434f-b4b2-fd7697d3cb73.png#averageHue=%23f4f3f1&clientId=uaf182df6-791c-4&from=paste&id=u1c6f0247&originHeight=186&originWidth=348&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=uc96334e7-9e3b-4a82-a238-ea47ad8c829&title=)![](https://cdn.nlark.com/yuque/0/2023/png/21870146/1692152788418-c7d25b66-2ae2-4e4d-a767-6df16c4ab29c.png#averageHue=%23f4f3f2&clientId=uaf182df6-791c-4&from=paste&id=u36ac14e1&originHeight=198&originWidth=338&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u7099a82a-d563-4efb-99f7-09c329fbdc2&title=)
2.对应是SQL语句

```sql
SELECT
 A.aname,
 B.hobby 
FROM
 A,
 B,
 AB 
WHERE
 A.id = AB.aid 
 AND B.id = AB.bid
```

3.对应的查询结果
