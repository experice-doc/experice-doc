### mySQL数据库主从配置
#### 数据库版本 机器配置 
```
主数据库所在的操作系统：centos
主数据库的版本：5.0
主数据库的ip地址：192.168.1.111

从数据库所在的操作系统：centos
从数据的版本：5.0
从数据库的ip地址：192.168.1.112
```

#### 确保主数据库与从数据库一模一样
```
例如：主数据库里的a的数据库里有b，c，d表，那从数据库里的就应该有一个模子刻出来的a的数据库和b，c，d表
```
#### 在主数据库上创建同步账号
```
GRANT REPLICATION SLAVE,FILE ON *.* TO 'mytest'@'192.168.1.112' IDENTIFIED BY '123456';
192.168.1.112：是运行使用该用户的ip地址
mytest：是新创建的用户名
123456：是新创建的用户名的密码
```
#### 配置主数据库的my.cnf
```
[mysqld]
server-id=1
log-bin=log
binlog-do-db=mytest//要同步的mytest数据库,要同步多个数据库，就多加几个replicate-db-db=数据库名

binlog-ignore-db=mysql  //要忽略的数据库
```

#### 配置从数据库的my.cnf
```
[mysqld]
server-id=2
master-host=192.168.1.111
master-user=mytest      　　//第一步创建账号的用户名
master-password=123456   //第一步创建账号的密码
master-port=3306
master-connect-retry=60
replicate-do-db=mytest //要同步的mstest数据库,要同步多个数据库，就多加几个replicate-db-db=数据库名

replicate-ignore-db=mysql　 //要忽略的数据库　
```

#### 验证是否成功
进入mysql，后输入命令:show slave status\G。如果slave_io_running和slave_sql_running都为yes，那么表明可以成功同

#### 测试同步数据
进入主数据库输入命令:insert into test(name) values('test');
然后进入从数据库输入命令：select * from test;
如果此时从数据库有获取到数据，说明同步成功了，主从也就实现了
