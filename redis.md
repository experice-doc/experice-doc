# Linux安装redis 


## 下载redis
    wget http://download.redis.io/releases/redis-2.8.12.tar.gz


## 解压安装
```
    tar xzf redis-2.8.12.tar.gz
    ln -s redis-2.6.14 redis #建立一个链接 
    cd redis  
    make PREFIX=/usr/local/redis install #安装到指定目录中

    注意上面的最后一行，我们通过PREFIX指定了安装的目录。如果make失败，一般是你们系统中还未安装gcc,那么可以通过yum安装： 
        yum install gcc
    安装完成后，继续执行make. 
    在安装redis成功后，你将可以在/usr/local/redis看到一个bin的目录，里面包括了以下文件：
        redis-benchmark  redis-check-aof  redis-check-dump  redis-cli  redis-server

```




