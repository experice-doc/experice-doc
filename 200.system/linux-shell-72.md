#73条日常Linux shell命令汇总

1.检查远程端口是否对bash开放：
    echo >/dev/tcp/8.8.8.8/53 && echo "open"

2.让进程转入后台：
    Ctrl + z

3、将进程转到前台：
    fg

4.产生随机的十六进制数，其中n是字符数：
    openssl rand -hex n

5.在当前shell里执行一个文件里的命令：
    source /home/user/file.name

6.截取前5个字符：
    ${variable:0:5}

7.SSH debug 模式:
    ssh -vvv user@ip_address

8.SSH with pem key:
    ssh user@ip_address -i key.pem

9.用wget抓取完整的网站目录结构，存放到本地目录中：
    wget -r --no-parent --reject "index.html*" http://hostname/ -P /home/user/dirs

10.一次创建多个目录：
    mkdir -p /home/user/{test,test1,test2}

11.列出包括子进程的进程树：
    ps axwef

12.创建 war 文件:
    jar -cvf name.war file

13.测试硬盘写入速度：
    dd if=/dev/zero of=/tmp/output.img bs=8k count=256k; rm -rf /tmp/output.img

14.测试硬盘读取速度：
    hdparm -Tt /dev/sda

15.获取文本的md5 hash：
    echo -n "text" | md5sum

16.检查xml格式：
    xmllint --noout file.xml

17.将tar.gz提取到新目录里：
    tar zxvf package.tar.gz -C new_dir

18.使用curl获取HTTP头信息：
    curl -I http://www.example.com

19.修改文件或目录的时间戳(YYMMDDhhmm):
    touch -t 0712250000 file

20.用wget命令执行ftp下载：
    wget -m ftp://username:password@hostname

21.生成随机密码(例子里是16个字符长):
    LANG=c < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-16};echo;

22.快速备份一个文件：
    cp some_file_name{,.bkp}

23.访问Windows共享目录：
    smbclient -U "DOMAIN\user" //dc.domain.com/share/test/dir

24.执行历史记录里的命令(这里是第100行):
    !100

25.解压:
    unzip package_name.zip -d dir_name

26.输入多行文字(CTRL + d 退出):
    cat > test.txt

27.创建空文件或清空一个现有文件：
    \> test.txt

28.与Ubuntu NTP server同步时间：
    ntpdate ntp.ubuntu.com

29.用netstat显示所有tcp4监听端口：
    netstat -lnt4 | awk '{print $4}' | cut -f2 -d: | grep -o '[0-9]*'

30.qcow2镜像文件转换：
    qemu-img convert -f qcow2 -O raw precise-server-cloudimg-amd64-disk1.img \precise-server-cloudimg-amd64-disk1.raw

31.重复运行文件，显示其输出（缺省是2秒一次）：
    watch ps -ef

32.所有用户列表：
    getent passwd

33.Mount root in read/write mode:
    mount -o remount,rw /

34.挂载一个目录（这是不能使用链接的情况）:
    mount --bind /source /destination

35.动态更新DNS server:
    nsupdate < <EOF
    update add $HOST 86400 A $IP
    send
    EOF

36.递归grep所有目录：
    grep -r "some_text" /path/to/dir

37.列出前10个最大的文件：
    lsof / | awk '{ if($7 > 1048576) print $7/1048576 "MB "$9 }' | sort -n -u | tail

38.显示剩余内存(MB):
    free -m | grep cache | awk '/[0-9]/{ print $4" MB" }'

39.打开Vim并跳到文件末：
    vim + some_file_name

40.Git 克隆指定分支(master):
    git clone git@github.com:name/app.git -b master

41.Git 切换到其它分支(develop):
    git checkout develop

42.Git 删除分支(myfeature):
    git branch -d myfeature

43.Git 删除远程分支
    git push origin :branchName

44.Git 将新分支推送到远程服务器：
    git push -u origin mynewfeature

45.打印历史记录中最后一次cat命令：
    !cat:p

46.运行历史记录里最后一次cat命令：
    !cat

47.找出/home/user下所有空子目录:
    find /home/user -maxdepth 1 -type d -empty

48.获取test.txt文件中第50-60行内容：
    < test.txt sed -n '50,60p'

49.运行最后一个命令(如果最后一个命令是mkdir /root/test, 下面将会运行: sudo mkdir /root/test)：
    sudo !!

50.创建临时RAM文件系统 – ramdisk (先创建/tmpram目录):
    mount -t tmpfs tmpfs /tmpram -o size=512m

51.Grep whole words:
    grep -w "name" test.txt

52.在需要提升权限的情况下往一个文件里追加文本：
    echo "some text" | sudo tee -a /path/file

53.列出所有kill signal参数:
    kill -l

54.在bash历史记录里禁止记录最后一次会话：
    kill -9 $$

55.扫描网络寻找开放的端口：
    nmap -p 8081 172.20.0.0/16

56.设置git email:
    git config --global user.email "me@example.com"

57.To sync with master if you have unpublished commits:
    git pull --rebase origin master

58.将所有文件名中含有”txt”的文件移入/home/user目录:
    find -iname "*txt*" -exec mv -v {} /home/user \;

59.将文件按行并列显示：
    paste test.txt test1.txt

60.shell里的进度条:
    pv data.log

61.使用netcat将数据发送到Graphite server:
    echo "hosts.sampleHost 10 `date +%s`" | nc 192.168.200.2 3000

62.将tabs转换成空格：
    expand test.txt > test1.txt

63.Skip bash history:
    < space >cmd

64.去之前的工作目录：
    cd -

65.拆分大体积的tar.gz文件(每个100MB)，然后合并回去：
    split –b 100m /path/to/large/archive /path/to/output/files
    cat files* > archive

66.使用curl获取HTTP status code:
    curl -sL -w "%{http_code}\\n" www.example.com -o /dev/null

67.设置root密码，强化MySQL安全安装:
    /usr/bin/mysql_secure_installation

68.当Ctrl + c不好使时:
    Ctrl + \

69.获取文件owner:
    stat -c %U file.txt

70.block设备列表：
    lsblk -f

71.找出文件名结尾有空格的文件：
    find . -type f -exec egrep -l " +$" {} \;

72.找出文件名有tab缩进符的文件
    find . -type f -exec egrep -l $'\t' {} \;

73.用”=”打印出横线:全选复制放进笔记
    printf '%100s\n' | tr ' ' =

74.查看所有正在运行的进程
    ps aux | less
    其中，
    -A：显示所有进程
    a：显示终端中包括其它用户的所有进程
    x：显示无控制终端的进程
    查看非root运行的进程 ps -U root -u root -N
    查看用户vivek运行的进程 ps -u vivek

75.crontab 查看服务器1分钟跑一次的脚本
    crontab -l | grep '^*/1' 

76.nohup命令后台执行脚本程序命令、守护进程
```
[tomener@localhost ~]$ python /data/python/server.py >python.log &

说明：
     1、 > 表示把标准输出（STDOUT）重定向到 那个文件，这里重定向到了python.log
     2、 & 表示在后台执行脚本
这样可以到达目的，但是，我们退出shell窗口的时候，必须用exit命令来退出，否则，退出之后，该进程也会随着shell的消失而消失（退出、关闭）



[tomener@localhost ~]$ nohup python /data/python/server.py > python.log3 2>&1 &
[1] 1885
[tomener@localhost ~]$ ps aux|grep python
tomener 1885  0.1  0.4  13120  4528 pts/0    S    15:48   0:00 python /data/python/server.py
tomener 1887  0.0  0.0   5980   752 pts/0    S+   15:48   0:00 grep python


说明：
1、1是标准输出（STDOUT）的文件描述符，2是标准错误（STDERR）的文件描述符
     1> python.log 简化为 > python.log，表示把标准输出重定向到python.log这个文件
2、2>&1 表示把标准错误重定向到标准输出，这里&1表示标准输出
     为什么需要将标准错误重定向到标准输出的原因，是因为标准错误没有缓冲区，而STDOUT有。
     这就会导致  commond > python.log  2> python.log 文件python.log被两次打开，而STDOUT和              STDERR将会竞争覆盖，这肯定不是我门想要的
3、好了，我们现在可以直接关闭shell窗口（我用的是SecureCRT，用的比较多的还有Xshell），而不用再输入exit这个命令来退出shell了


--------------------------------------------------------------------------------------
现在当我们，直接关闭shell窗口，再连接上服务器，查看Python的进程
发现，进程还在哦~~
嘿嘿，这就是nohup的好处。

[tomener@localhost ~]$ ps aux|grep python
tomener 1885  0.1  0.4  13120  4528 pts/0    S    15:48   0:00 python /data/python/server.py
tomener 1887  0.0  0.0   5980   752 pts/0    S+   15:48   0:00 grep python


```

77 Linux查看物理CPU个数、核数、逻辑CPU个数
```
# 总核数 = 物理CPU个数 X 每颗物理CPU的核数 
# 总逻辑CPU数 = 物理CPU个数 X 每颗物理CPU的核数 X 超线程数

# 查看物理CPU个数
cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l

# 查看每个物理CPU中core的个数(即核数)
cat /proc/cpuinfo| grep "cpu cores"| uniq

# 查看逻辑CPU的个数
cat /proc/cpuinfo| grep "processor"| wc -l
```
