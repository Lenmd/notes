```sql
-- 查看InnoDB引擎下所有事物信息
SELECT * FROM information_schema.innodb_trx ORDER BY trx_started; 
trx_id 事务id
trx_state `RUNNING`、`LOCK WAIT`、`COMMITTING`
trx_started 事物开始时间
trx_mysql_thread_id 
trx_rows_modified 事物修改行数，可判断大事务

-- 删除
kill  634031;  trx_mysql_thread_id
```