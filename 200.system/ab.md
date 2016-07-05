# ab 压力测试

#####ab是apache自带的一个很好用的压力测试工具，当安装完apache的时候，就可以在bin下面找到ab

### 我们可以模拟100个并发用户，对一个页面发送1000个请求
    ./ab -n1000 -c100  http://127.0.0.1/a.php
    其中-n代表请求数，-c代表并发数

### 返回结果
```
首先是apache的版本信息 
This is ApacheBench, Version 2.3 <Revision:655654Revision:655654> 
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/ 
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking vm1.jianfeng.com (be patient)

Server Software:        Apache/2.2.19    ##apache版本 
Server Hostname:        vm1.jianfeng.com   ##请求的机子 
Server Port:            80 ##请求端口

Document Path:          /a.html 
Document Length:        25 bytes  ##页面长度

Concurrency Level:      100  ##并发数 
Time taken for tests:   0.273 seconds  ##共使用了多少时间 
Complete requests:      1000   ##请求数 
Failed requests:        0   ##失败请求 
Write errors:           0   

Total transferred:      275000 bytes  ##总共传输字节数，包含http的头信息等 
HTML transferred:       25000 bytes  ##html字节数，实际的页面传递字节数 
Requests per second:    3661.60 [#/sec] (mean)  ##每秒多少请求，这个是非常重要的参数数值，服务器的吞吐量 
Time per request:       27.310 [ms] (mean)  ##用户平均请求等待时间 
Time per request:       0.273 [ms] (mean, across all concurrent requests)  ##服务器平均处理时间，也就是服务器吞吐量的倒数 
Transfer rate:          983.34 [Kbytes/sec] received  ##每秒获取的数据长度

Connection Times (ms) 
              min  mean[+/-sd] median   max 
Connect:        0    1   2.3      0      16 
Processing:     6   25   3.2     25      32 
Waiting:        5   24   3.2     25      32 
Total:          6   25   4.0     25      48

Percentage of the requests served within a certain time (ms) 
  50%     25  ## 50%的请求在25ms内返回 
  66%     26  ## 60%的请求在26ms内返回 
  75%     26 
  80%     26 
  90%     27 
  95%     31 
  98%     38 
  99%     43 
100%     48 (longest request)
```