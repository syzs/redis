# redis
特性：复制、持久化、分片<br>
内存数据库、不使用表

## redis相比memcached有哪些优势？
  * memcached所有的值均是简单的字符串，redis作为其替代者，支持更为丰富的数据类型
  * redis的速度比memcached快很多
  * redis可以持久化其数据

## 支持数据结构
[详情](http://redisdoc.com/)

|结构类型	|常用命令	|描述	|
| ------------- |:-------------:| -----:|
| string(字符串)			|  |  |
| list(链表)				|  |  |
| set(无序不可重复集合)	|  |  |
| zset(有序集合)			|  |  |
| hash(散列表)				|  |  |


## 持久化：
### RDB: redis database
* 在指定时间间隔内，将内存中的数据作为一个快照文件（snapshot）写入到磁盘，读取的时候也是直接读取snapshot文件到内存中
* redis单独创建（fork）一个进程来持久化，会先将数据写入临时文件中，待上次持久化结束后，会将该临时文件替换上次持久化文件，比aof高效，但是最后一次数据可能会丢失

### AOF: appendof files

* 以日志形式记录每个写操作，启动时通过日志恢复操作
* 开启AOF：默认不开启，进入redis.conf找到appendonly yes打开
* 修复AOF：redis-check-aof –fix appendonly.aof
* 同步频率：每秒记录一次，如果宕机该秒记可能失效
* Rewrite：bgrewriteaof 因为日志是追加方式，文件会越来越大，当超过了设置的阈值时，日志文件会压缩，保留仅可以恢复的日志

### RDB 与 AOF 优劣对比

## 淘汰数据

redis 内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略（回收策略）。redis 提供 6种数据淘汰策略：

* volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
* volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
* volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
* allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰
* allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
* no-enviction（驱逐）：禁止驱逐数据