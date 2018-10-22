### Oracle  数据库查询表中约束

*  sql 查询 表中的约束   table_name 为 表名称

```javascript
select table_name,constraint_name,constraint_type from user_constraints
where table_name='Table Name ' ;
```

* sql 查询 索引

  ```javascript
  //   Table_NAME 为表名称 不加 则查询所有索引
  select s.*from user_indexes s where s.TABLE_NAME  ='Table_Name'  ;
  
  // 删除索引  indexsname 为索引名称
  drop index indexesname
  ```

  