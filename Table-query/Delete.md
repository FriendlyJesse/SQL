# 2.4. 删除数据

```sql
DELETE FROM 表名 WHERE 字段名=字段值;
-- 清空表数据
DELETE FROM 表名;
```

如果不加入条件，则会清空表中所有数据。

## 2.4.1. Soft deletes

软删除是数据库中使用的一种技术，用于将记录标记为已删除，而无需从数据库中物理地删除它们。这种方法允许以后在需要时恢复或恢复已删除的数据。

**example:**

```sql
UPDATE table_name SET is_delete=true WHERE id=? AND is_delete=false
```
