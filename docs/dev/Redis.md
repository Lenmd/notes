## 命令
```
基础操作
ping
auth <password>
select <db index>
dbsize
flushdb
flushall
keys <pattern>
exists <key>
del <key>
expire <key> <seconds>
ttl <key>
type <key>

字符串
set <key> <value>
get <key>
setex <key> <ttl> <val>
incr <key>
decr <key>
append <key> <value>

哈希表
hset <key> <field> <value>
hget <key> <field>
hmset <key> <field1> <v1> ...
hmget <key> <field1> <field2>
hgetall <key>
hdel <key> <field>
hexists <key> <field>

List
lpush <key> <value>
rpush <key> <value>
lpop <key>
rpop <key>

Set
sadd <key> <value>
srem <key> <value>
smembers <key>
```

常用

```bash

# 设置值并设置过期时间
setex token:user123 60 "abc123"

keys user:*

# 批量获取key
mget key1 key2 key3 ...

#hash类型
hmget key field1 field2 ...

```