#ngxtop：在命令行实时监控 Nginx 的神器

##linux 安装
```
$ sudo pip install ngxtop
```

##mac 安装
```
1:sudo easy_install pip
2:sudo easy_install ngxtop
```

##ngxtop 使用
```
1:ngxtop [options]
2:ngxtop [options] (print|top|avg|sum) <var>
3:ngxtop info
```
###参数
```
-l : 指定日志文件的完整路径 (Nginx 或 Apache2)
-f : 日志格式
--no-follow: 处理当前已经写入的日志文件，而不是实时处理新添加到日志文件的日志
-t : 更新频率
-n : 显示行号
-o : 排序规则(默认是访问计数)
-a ..., --a ...: 添加表达式(一般是聚合表达式如： sum, avg, min, max 等)到输出中。
-v: 输出详细信息
-i : 只处理符合规则的记录
```
###内置变量
```
odybytessend
http_referer
httpuseragent
remote_addr
remote_user
request
status
time_local
```

###使用 ngxtop 监控 Nginx
```
ngxtop 默认会从其配置文件 (/etc/nginx/nginx.conf) 中查找 Nginx 日志的地址。所以，监控 Nginx ，运行以下命令即可：
$ ngxtop
这将会列出10个 Nginx 服务，按请求数量排序。
```

### 显示前20个最频繁的请求
```
$ ngxtop -n 20
```

### 获取Nginx基本信息
```
$ ngxtop info
```

### 使用 "print" 命令显示自定义请求
```
$ ngxtop print request http_user_agent remote_addr
```

### 显示请求最多的客户端IP地址
```
$ ngxtop top remote_addr
```

### 显示状态码是404的请求
```
$ ngxtop -i 'status == 404' print request status
```

### 处理其他的日志文件
```
比如 Apache 的访问文件。使用以下命令监控 Apache 服务器:
$ tail -f /var/log/apache2/access.log | ngxtop -f common
```