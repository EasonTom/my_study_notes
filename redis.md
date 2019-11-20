# redis基础操作

## 1 redis简介

Redis是一款基于内存的高速缓存数据库，全称为：Remote Dictionary Server，使用C语言编写。Redis是一个key-value存储系统，它支持丰富的数据类型，如：string、list、set、zset(sorted set)、hash。

Redis特点:

- 快速响应，数据量小

- Redis以内存作为数据存储介质，所以读写数据的效率极高，远远超过数据库。

Redis应用场景:

因为Redis交换数据快，所以在服务器中常用来存储一些需要频繁调取的数据，这样可以大大节省系统直接读取磁盘来获得数据的I/O开销，更重要的是可以极大提升速度。

将这种热点数据存到Redis（内存）中，要用的时候，直接从内存取，极大的提高了速度和节约了服务器的开销。

## 2 安装和使用

### 2.1 ubuntu安装redis:

```
sudo apt-get update
sudo apt-get install redis-server
```

打开数据库，默认端口6379

```
tys@tys:~$ redis-cli
127.0.0.1:6379> 
```

输入`exit`或者快捷键`ctrl+d`退出数据库。

清空数据库：

```
flushdb
```

### 2.2 redhat安装redis(linux通用):

1.获取redis资源


    wget http://download.redis.io/releases/redis-4.0.8.tar.gz

2.解压


    tar xzvf redis-4.0.8.tar.gz

3.安装

    cd redis-4.0.8

    make

    cd src

    make install PREFIX=/usr/local/redis

4.移动配置文件到安装目录下

    cd ../

    mkdir /usr/local/redis/etc

    mv redis.conf /usr/local/redis/etc

 5.配置redis为后台启动

    vi /usr/local/redis/etc/redis.conf //将daemonize no 改成daemonize yes

6.将redis加入到开机启动

    vi /etc/rc.local //在里面添加内容：/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf (意思就是开机调用这段开启redis的命令)

7.开启redis

    /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf 

 

常用命令　　

    redis-server /usr/local/redis/etc/redis.conf //启动redis

    pkill redis  //停止redis

    #卸载redis：

    rm -rf /usr/local/redis //删除安装目录

    rm -rf /usr/bin/redis-* //删除所有redis相关命令脚本

    rm -rf /root/download/redis-4.0.4 //删除redis解压文件夹


## 3 数据类型

### 3.1 字符串

**可以为整形、浮点型和字符串，统称为元素**

```
set key value  # 设置，字符串可以不加引号，系统会自动加上
get key  # 获取，如果键不存在则返回空
```

**key是唯一的,不能用同一个key 不然就会覆盖**



过期时间：

```
ttl key  # 查看过期时间，-1表示永久，-2代表不存在
expire key seconds  #设置过期时间，单位位秒
set key value ex seconds  # 设置键值对的同时设置过期时间
set key seconds value  # 设置键值对的同时设置过期时间
```

追加：

```
append key value  # 在value追加在原key的值后面形成新的值
```

多个操作：

```
mset key1 value1 key2 value2 ...
mget key1 key2 ...
```

key操作：

```
keys *  # 查看所有的key
keys *p  # 查看p开头的key
del key  # 删除key
exists key  # 判断key是否存在，存在返回1，不存在返回
type key  # 查看key的类型
```

**redis使用nil表示空值**

运算：

```
set key 1  # 自动识别整数
incr key  # 加1
decr key  # 减1
incrby key increment  # 加整数
decrby key increment  # 减整数
```

### 3.2 list

**实现队列，元素不唯一，先入先出原则**

基本操作

```
lpush key value1,value2,...,valuen  # 左添加
rpush key value1,value2,...,valuen  # 右添加
lrange key start stop  # 查看列表的元素，需要指定索引，0 -1表示全部
llen key  # 获取列表长度
lindex key index  # 获取列表索引为index的元素，索引从0开始

# 删除
lpop key  # 弹出左边第一个元素
rpop key  # 弹出右边第一个元素
# 指定删除
lrem key count value  
# count指定删除数量，value指定要删除的元素
# count>0，从左往右删除
# count<0，从右往左删除
# count=0，全部删除
```

### 3.3 Hash

**Hash是一个string类型的field和value的映射表，特别适合用于存储对象**

```
key:(field.value)
```

**hash的key必须是唯一的**

```
hset key field value  # 设置
hget key field  # 获取
hdel key field  # 删除
hmset key field1 value1 field2 value2  # 设置多个
hmget key field1 field2  # 获取多个
hgetall key  # 获取全部的field-value
hkeys key  # 获取所有的field
hvals key  # 获取所有的value
hlen key  # 获取field长度
type key  # 获取field类型
```

### 3.4 set

无序且唯一

```
sadd key value [value] 设置	
	
smembers set 获取	
	
	
srem set value 指定删除	
spop set  随机删除	
	
smove set1 set2 value 移动值	
sismember set value 判断值是否存在	
sinter set1 set2 交集	
sunion set1 set2 并集	
sdiff set1 set2 差集	
sinterstore new_set set1 set2 交集合并到新集合
sunionstore new_set set1 set2并集合并	
sdiffstore new_set set1 set2差集合并
scard set 获取个数
srandmember set 随机返回一个元素	

```

### 3.5 sorted set

```
设置，score代表权值，排序按照权值
zadd key score member [score value]

获取	
正序	
zrange zset start stop
倒序	
zrevrange zset start stop
	
删除	
zrem zset value
	
	
正序查看索引位置，从0开始
zrank zset value
反序	
zrevrank zset value

查看元素数量	
zcrad zset

查看score值	
zscore zset value


```

