## Oracle 导出 数据表 结构 
####  SQL 代码
* #####  以下有注释的 可以根据个人需要 取消注释
```
SELECT 
      --  COL.TABLE_NAME,
      --   TT.COMMENTS,
       COL.COLUMN_NAME AS 字段名称,
       CASE
       WHEN PKCOL.COLUMN_POSITION > 0 THEN
          '√'
       ELSE
          ''
       END AS 是否主键,
       COL.DATA_TYPE AS 字段类型,
       COL.DATA_LENGTH    占用字节数,
       -- COL.DATA_PRECISION AS PRECI,
       -- COL.DATA_SCALE     AS SCALE,
       CASE
       WHEN COL.NULLABLE = 'Y' THEN
          '√'
         ELSE
          ''
       END AS 是否为空,
       -- COL.DATA_DEFAULT AS DEFAULTVAL,
       CCOM.COMMENTS    AS 字段描述 
FROM 
       USER_TAB_COLUMNS COL,
       USER_COL_COMMENTS CCOM,
       (SELECT AA.TABLE_NAME,
               AA.INDEX_NAME,
               AA.COLUMN_NAME,
               AA.COLUMN_POSITION
          FROM USER_IND_COLUMNS AA, USER_CONSTRAINTS BB
         WHERE BB.CONSTRAINT_TYPE = 'P'
           AND AA.TABLE_NAME = BB.TABLE_NAME
           AND AA.INDEX_NAME = BB.CONSTRAINT_NAME
        ) PKCOL,
       USER_TAB_COMMENTS TT
WHERE COL.TABLE_NAME = CCOM.TABLE_NAME
      AND COL.COLUMN_NAME = CCOM.COLUMN_NAME
      AND COL.TABLE_NAME = TT.TABLE_NAME(+)
      AND COL.COLUMN_NAME = PKCOL.COLUMN_NAME(+)
      AND COL.TABLE_NAME = PKCOL.TABLE_NAME(+)
and   COL.TABLE_NAME ='RQ_STOCKTYPE'
ORDER BY COL.TABLE_NAME,col.column_id;


```
