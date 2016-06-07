# Linux安装go

```
1.首先，检查下自己操作系统的位数，使用uname -a 来查看
2.如果是64位，则会显示x86_64字样，如果是32位，则会显示i686字样，
3.然后到https://code.google.com/p/go/downloads/list  这里找对应的包下载

  tar -zxvf  go1.1.linux-386.tar.gz 
  cp -R go/ /usr/local/go

4.接下来要设置的就是环境变量了
  linux的环境变量分两种，临时变量和永久的变量
  1.vi /etc/profile 文件设置的变量是对所有用户永久有效
  2.vi /用户目录/.bash_profile 文件是对某个用户永久有效
  3.使用export，只是对当前shell有效，shell关闭则失效

  推荐第一种 vi /etc/profile 在文件末尾加入：
  export GOROOT=/usr/local/go
  export GOBIN=$GOROOT/bin
  export GOPATH=/root/lvxinxin
  export PATH=$PATH:$GOBIN:$GOPATH


断开shell重新连接或者是source /etc/profile 立刻生效
然后直接使用 go version 会显示，例：
go version go1.1 linux/386
也可以使用go env来查看其它的变量
    GOARCH="386"
    GOBIN="/usr/local/go/bin"
    GOCHAR="8"
    GOEXE=""
    GOHOSTARCH="386"
    GOHOSTOS="linux"
    GOOS="linux"
    GOPATH="/root/go"
    GORACE=""
    GOROOT="/usr/local/go"
    GOTOOLDIR="/usr/local/go/pkg/tool/linux_386"
    CC="gcc"
    GOGCCFLAGS="-g -O2 -fPIC -m32 -pthread"
    CGO_ENABLED="1"


1.Test your installation 
        
    hello.go:
    package main
    import "fmt"
    func main() {
        fmt.Printf("hello, world\n")
    }
2.$ go run hello.go
hello, world
``` 