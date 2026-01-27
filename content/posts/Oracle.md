---
title: "Oracle命令"
subtitle: ""
date: '2024-01-28T03:47:00+08:00'
lastmod: '2026-01-27T11:01:00+08:00'
draft: false
categories: ['学习']
tags: ['Oracle','命令']
---



### 1、查看表中索引
1. 显示索引名称，索引作用的列
```sql
select * from user_ind_columns where table_name='表名';
```
2. 显示索引的具体信息（创建时间、类型、内存大小）
```sql
select * from all_indexes where table_name='表名';
```
### 2、删除序列
```sql
delete sequence 序列名;
```
### 3、查看存储过程/触发器编译错误信息
```sql
select * from SYS.USER_ERRORS where NAME=upper('出错的名称（触发器/存储过程m）');
```
### 4、约束条件相关操作
1. 查看表的约束条件
```sql
select * from user_constraints where table_name='表名';
```
2. 删除约束条件
```sql
alter table 表名 drop constraint 约束条件名;
```
3. 失效约束条件
```sql
alter table 表名 disable constraint 约束条件名;
```
4. 生效约束条件
```sql
alter table 表名 enable constraint 约束条件名;
```
### 5、查看所有存储过程、触发器、视图、表
```sql
select * From user_triggers; --所有触发器
select * From user_procedures; --所有存储过程
select * From user_views; --所有视图
select * From user_tables; --所有表
```
### 6、查看当前数据库连接的sid实例
```sql
select instance_name from v$instance;
```

### 7、查看用户拥有的表空间

```sql
select tablespace_name from user_tablespaces;
```

