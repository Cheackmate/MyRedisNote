# Redsi入门介绍



## 入门概述



### 是什么

Redis:Remote Dictionary server(远程字典服务器)

是一个开源免费的，用c语言编写的，遵守BSD协议的，一个高性能的key/vale分布式内存数据库基于内存运行并支持吃花的nosql数据库，是当前最热门的nosql数据库之一，也被人们称为数据结构服务器

Redis与其他key-value缓存产品有以下三个特点

### Redis在Linux下安装过程

#### 1.使用wget下载对应的redis包

>  wget https://download.redis.io/releases/redis-6.0.9.tar.gz

#### 2.使用tar解压

> tar -zxvf redis-6.0.9.tar.gz

解压之后出现了一个目录cd到redis-6.0.9目录中

#### 3.安装gcc

> yum -y install gcc-c++

如果没有gcc安装可能会出现错误因为redis是c语言编写的，没有c原因的环境不能远行

#### 4.安装完成后安装vim

> yum -y install vim

vim编辑器非常好用

#### 5.在redis目录中进行安装

> make install

安装完成之后会在/usr/local/bin目中生成对应的安装文件

#### 6.保存配置文件

在自己喜欢的位置创建一个redis.conf的备份文件

当前位置 /redis-6.0.9目录下

> mkdir /myredis
>
> cp redis.conf  /myredis

#### 7.修改配置文件

> vim /myredis/redis.conf

shif+4 == $ 跳到行尾

> daemonize  yes

#### 8.切换目录然后启动redis

> cd /usr/local/bin
>
> redis-server /myredis/redis.conf

#### 9.启动redis

> redis-cli -p 6379

#### 10.结束redis

> shutdown
>
> exit

### Redis在Windows下安装

[Redis X.X.X for Windows](https://github.com/tporadowski/redis/releases)

大多数企业是使用Linux的Redis，Windows仅供学习。



# **一.Redis临时服务**

**1**.打开cmd，进入到刚才解压到的目录，启动临时服务：redis-server.exe redis.windows.conf (备注：通过这个命令，会创建Redis临时服务，不会在window Service列表出现Redis服务名称和状态，此窗口关闭，服务会自动关闭。)

![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123134651982-153935545.png)

 

**2**.打开另一个cmd窗口，客户端调用：redis-cli.exe -h 127.0.0.1 -p 6379

 ![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123135300435-372346969.png)

 

#  **二.Redis自定义windows服务安装**

 **1**.进入Redis安装包目录，安装服务：redis-server.exe --service-install redis.windows.conf --service-name redisserver1 --loglevel verbose

![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123140253097-143132736.png)

 

 win+r -> services.msc,可以看到服务安装成功

安装服务：redis-server.exe --service-install redis.windows.conf --service-name redisserver1 --loglevel verbose

启动服务：redis-server.exe --service-start --service-name redisserver1

停止服务：redis-server.exe --service-stop --service-name redisserver1

卸载服务：redis-server.exe --service-uninstall--service-name redisserver1



# 三. 主从服务器

将d盘下新建一个文件夹叫redis2，把redis文件夹的东西拷贝到redis2文件夹下，将redis-windows.conf配置文件中的ip 和端口号改一下，然后按照上面的步骤按照一个服务即可

 ![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123141854804-566323183.png)

 

 

 使用redis桌面管理器（下载地址:https://redisdesktop.com/），看到两个redis库

 ![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123142053571-1209756947.png)

 

设置密码把 #requirepass foobared 的#号去掉改为自己的密码即可

设置好保存后，若要使设置起作用，需要重启redis服务

端口号和ip同理

![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123170250904-844974463.png)

 

 ![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123170315882-1565385270.png)

 

重启后需要输入密码 

![img](https://img2018.cnblogs.com/blog/779272/201901/779272-20190123170957172-142182562.png)

 



### Redis杂项基础知识

Redis默认有16个库

> select 0-15     可以进行切换库的使用

设置键值对

> set key1 value1

清除数据

> flushdb 清除当前库的数据
>
> flushall清除所有16个库的数据

查找数据

> key *
>
> key k?
>
> key k ??

查看当前数据库数据量

> dbsize

默认开放端口

6379

### Redis基础操作

#### 1.常用

> keys * 查找所有键值
>
> exists keyname   查看某个key是否存在
>
> move  key db    移动某个数据到其他库
>
> expire key  设置某个数据超时时间，超时后会自动删除。
>
> ttl  key 查询多少秒后过期 -1表示永远不过期，-2表示已经过期
>
> type  key 查看key是什么类型
>
> set  key value   如果key有数值，覆盖



#### 2.字符串常用

基础操作

> set/get/del/append/strlen   设置/获取/删除/追加/长度
>
> incr/decr/incrby/decrby   数值计算加多少，减多少
>
> getrange  k3   0 -1      获取之情范围内的数据
>
> setrange  k3 0 XXX   返回修改数据
>
> setex k2 60 v2设置一个key可以存在的时间，setnx设置key如果不存在就设置存在就不设置，
>
> mset/mget/msetnx  多个键值对的同时获取，修改，和如果有存在的有不存在的就不成功
>
> getset  先获取在设置

#### 3.List的使用

基础操作

> lpush/rpush/lrange    左侧插入/右侧插入/范围查看mset
>
> lpop/rpop    左侧删除/右侧删除
>
> lindex	按照索引下标获取元素
>
> llen   长度
>
> lrem  key  n value      删除 key的 n 个 值
>
> ltrim key  start   end					开始index结束index,截取指定范围内的数据
>
> rpoplpush   				删除一个列表的最右端的数据，复制到另一个列表的最左端
>
> lset key index vlaue 	设置某个位置的数据为某个值
>
> linsert  key before/after 	值1 值2   在某个值的前面或者后面插入数据

性能总结：

- 它是一个字符串链表，left、right都可以插入添加；
- 如果键不存在，创建新的链表；
- 如果键已存在，新增内容；
- 如果值全移除，对应的键也就消失了。
- 链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了。

#### 3.Set的基础操作

> sadd/smembers/sismember
>
> scard，获取集合里面的元素个数
>
> srem key value 删除集合中元素
>
> srandmember key 某个整数(随机出几个数)
>
> spop key 随机出栈
>
> smove key1 key2 v  将key1中 v剪切到key2

- 数学集合类
  - 差集：sdiff
  - 交集：sinter
  - 并集：sunion

#### 4.Hash的基础操作

> hset/hget/hmset/hmget/hgetall/hdel
>
> hset hset1 k1 v1  / hget hset1 k1 / hmset k1 v1 k2 v2 k3 v3/hmget k1 k2 k3/hgetall hset1/hdel hset1 k1
>
> hlen
>
> hexists key 在key里面的某个值的key
>
> hkeys/hvals
>
> hincrby/hincrbyfloat
>
> hsetnx

####  5.ZSET的技术操作

> zadd/zrange
>
> - Withscores
>
> zrangebyscore key 开始score 结束score
>
> - withscores
> - ( 不包含
> - Limit 作用是返回限制
>   - limit 开始下标步 多少步
>
> zrem key 某score下对应的value值，作用是删除元素
>
> zcard/zcount key score区间/zrank key values值，作用是获得下标值/zscore key 对应值,获得分数
>
> zrevrank key values值，作用是逆序获得下标值
>
> zrevrange
>
> zrevrangebyscore key 结束score 开始score