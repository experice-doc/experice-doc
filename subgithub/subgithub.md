## github 便捷提交
```
在github.com上 建立了一个小项目，可是在每次push  的时候，都要输入用户名和密码，很是麻烦

原因是使用了https方式 push

在termail里边 输入  Git remote -v 

可以看到形如一下的返回结果

origin https://github.com/yuquan0821/demo.git (fetch)

origin https://github.com/yuquan0821/demo.git (push)

下面把它换成ssh方式的。

1. git remote rm origin
2. git remote add origin git@github.com:yuquan0821/demo.git
3. git push origin 
```

mac下面记住git用户名密码
```
在Mac OS X中这个操作竟然如此简单。只需在Terminal中输入如下的命令：

git config --global credential.helper osxkeychain
然后在git操作时只要输入一次用户名与密码，以后就不用输入了。
```
