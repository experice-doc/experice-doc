#nginx 配置优化

### 基本的 (优化过的)配置
```
我们将修改的唯一文件是nginx.conf，其中包含Nginx不同模块的所有设置。你应该能够在服务器的/etc/nginx目录中找到nginx.conf。首先，我们将谈论一些全局设置，然后按文件中的模块挨个来，谈一下哪些设置能够让你在大量客户端访问时拥有良好的性能，为什么它们会提高性能。本文的结尾有一个完整的配置文件。
```

### 高层的配置
```
nginx.conf文件中，Nginx中有少数的几个高级配置在模块部分之上。
    1. user www-data; 
    2. pid /var/run/nginx.pid; 
    3. worker_processes auto; 
    4. worker_rlimit_nofile 100000; 

user和pid应该按默认设置 - 我们不会更改这些内容，因为更改与否没有什么不同。
```

#### worker_processes
    定义了nginx对外提供web服务时的worker进程数。最优值取决于许多因素，包括（但不限于）CPU核的数量、存储数据的硬盘数量及负载模式。不能确定的时候，将其设置为可用的CPU内核数将是一个好的开始（设置为“auto”将尝试自动检测它）。

#### worker_rlimit_nofile
    更改worker进程的最大打开文件数限制。如果没设置的话，这个值为操作系统的限制。设置后你的操作系统和Nginx可以处理比“ulimit -a”更多的文件，所以把这个值设高，这样nginx就不会有“too many open files”问题了。用ulimit -a 查看你系统的设置

#### Events
```
    events模块中包含nginx中所有处理连接的设置。
        events { 
           worker_connections 2048; 
           multi_accept on; 
           use epoll; 
        } 
```

#### worker_connections
    设置可由一个worker进程同时打开的最大连接数。如果设置了上面提到的worker_rlimit_nofile，我们可以将这个值设得很高。记住，最大客户数也由系统的可用socket连接数限制（~ 64K），所以设置不切实际的高没什么好处。

#### multi_accept
    告诉nginx收到一个新连接通知后接受尽可能多的连接。

#### use
    设置用于复用客户端线程的轮询方法。如果你使用Linux 2.6+，你应该使用epoll。如果你使用*BSD，你应该使用kqueue。（值得注意的是如果你不知道Nginx该使用哪种轮询方法的话，它会选择一个最适合你操作系统的）

#### HTTP 模块
```
    HTTP模块控制着nginx http处理的所有核心特性。因为这里只有很少的配置，所以我们只节选配置的一小部分。所有这些设置都应该在http模块中，甚至你不会特别的注意到这段设置。
    http { 
        server_tokens off; 
        sendfile on; 
        tcp_nopush on; 
        tcp_nodelay on; 
        ... 
    } 
```

#### server_tokens
    并不会让nginx执行的速度更快，但它可以关闭在错误页面中的nginx版本数字，这样对于安全性是有好处的.

#### sendfile
    可以让sendfile()发挥作用。sendfile()可以在磁盘和TCP socket之间互相拷贝数据(或任意两个文件描述符)。Pre-sendfile是传送数据之前在用户空间申请数据缓冲区。之后用read()将数据从文件拷贝到这个缓冲区，write()将缓冲区数据写入网络。sendfile()是立即将数据从磁盘读到OS缓存。因为这种拷贝是在内核完成的，sendfile()要比组合read()和write()以及打开关闭丢弃缓冲更加有效(更多有关于sendfile)。

#### tcp_nopush
    告诉nginx在一个数据包里发送所有头文件，而不一个接一个的发送。

#### tcp_nodelay
    告诉nginx不要缓存数据，而是一段一段的发送--当需要及时发送数据时，就应该给应用设置这个属性，这样发送一小块数据信息时就不能立即得到返回值。
    access_log off; 
    error_log /var/log/nginx/error.log crit;

#### access_log
    设置nginx是否将存储访问日志。关闭这个选项可以让读取磁盘IO操作更快(aka,YOLO)