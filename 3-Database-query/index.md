- [3. Database query](#3-database-query)
  - [3.1. 创建数据库](#31-创建数据库)
  - [3.2. 删除数据库](#32-删除数据库)
  - [3.3. 查看版本](#33-查看版本)
  - [3.4. PostgreSQL](#34-postgresql)

# 3. Database query

## 3.1. 创建数据库

```sql
CREATE DATABASE mydb;
```

## 3.2. 删除数据库

```sql
DROP DATABASE mydb;
```

## 3.3. 查看版本

```sql
SELECT VERSION();
```

## 3.4. PostgreSQL

```bash
\l # 数据库列表
\q # 离开
\c 数据库名 # 进入数据库
\d 表名 # 查看表结构
```
