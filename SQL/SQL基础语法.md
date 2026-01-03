---
tags:
  - SQL
date: 2026-01-02
---

# 语句以分号分隔

# Create
```SQL
create database NAME charset UTF8;
```

### 创建数据库

```SQL
create table Skyline
(
	id int;
	name char();
	sex char();
	Birthday date();
	job varchar();
)
```
### 创建表

# rename
重命名
```SQL
rename table skyline to xenonphia;
```

# use
进入/使用
```SQL
use (database_name);
```

# drop
删除
```SQL
drop database skyline;
```

# insert
插入数据
```SQL
insert into skyline
values(xxx,xxx,xxx);
```

# alter
更改属性
```SQL
alter table skyline add salary decimal(10,2);
alter table skyline modify column salary decimal(8,1); 
```

# select and union
### 后续使用

# order by and group by
排序

