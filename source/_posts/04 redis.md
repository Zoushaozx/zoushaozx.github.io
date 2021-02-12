安装redis

```
wget http://download.redis.io/releases/redis-6.0.8.tar.gz
tar xzf redis-6.0.8.tar.gz
cd redis-6.0.8
make
cd src
启动 redis 使用的是默认配置
	./redis-server
自己的配置文件
./redis-server ../redis.conf
```

