# 目的

如何使用服务器连接本地网络或者VPN

# 参考资料

1. [ssh tunneling - Give server access to Internet, via client connecting by SSH - Unix &amp; Linux Stack Exchange](https://unix.stackexchange.com/questions/116191/give-server-access-to-internet-via-client-connecting-by-ssh)
2. [在Windows机器搭建squid代理\_squid windows\_(～￣▽￣)～凤凰涅槃的博客-CSDN博客](https://blog.csdn.net/qq_42704442/article/details/127746279)

  Host A:本地windows
  Host B:远程节点

# 1. 下载

首先，在参考资料2中下载squid并使其运行
https://packages.diladele.com/squid/4.14/squid.msi

# 2. 修改本地squid配置

接着，根据参考资料1，

进入 `/etc/squid/squid.conf`文件注释 `http_access deny all `并添加 `http_access allow all`

# 3. 代理

若不使用代理则跳过这一条也可运行，且在不使用代理的情况下，需要注释以下配置

如果主机A本身使用一些代理' 10.140.78.130:8080 '(就像例子一样)连接到互联网，则需也添加代理到' /etc/squid/squid.conf '，如下所示:

```config
refresh_pattern (Release|Packages(.gz)*)$ 0 20% 2880
cache_peer 10.140.78.130 parent 8080 0 no-query default
never_direct allow all
```

这里我使用的是clash，静态ip，因此第二行修改为
`cache_peer 127.0.0.1 parent 7890 0 no-query default`


# 4. 修改服务器bashrc

Host B 上:将下面的语句加入bashrc并source。原教程中使用的是environment，没有root实现不了。

```
export http_proxy=http://127.0.0.1:3129
export https_proxy=http://127.0.0.1:3129
```

# 5. 启动本地squid

安装后有Squid Server Tray的快捷方式，双击启动
我发现只需要打开一次squid即可，之后即使关闭，squid.exe仍然运行在后台，不影响连接

# 6. 服务器添加端口转发即可运行

- 在vscode中，在端口中加上3128即可
  或者，在ssh默认配置文件中，可以添加这一行以永久forward这个端口
  `RemoteForward 3129 localhost:3128`
  - 添加完后需要重启vscode，此后只需要开启Squid即可
- 如果没有vscode
  在本地电脑的终端中输入 `ssh -R 3129:localhost:3128 user@10.168.203.60`，此后每次想连接都需要提前输入此命令

# 7. 测试

```shell
wget www.google.com # 有代理
wget www.baidu.com  # 无代理
```

显示

> --2023-06-01 22:00:27--  http://www.google.com/
> Connecting to 127.0.0.1:3128... connected.
> Proxy request sent, awaiting response... 302 Found
> ...
> Saving to: ‘index.html’
>
> index.html                        [ <=>                                              ]  16.88K  --.-KB/s    in 0.07s
>
> 2023-06-01 22:00:30 (253 KB/s) - ‘index.html’ saved [17281]

# 8. 一些可能的问题以及debug办法

- vscode中出现`Warning: remote port forwarding failed for listen port 3129`
  - 有时会遇到端口连接失败的问题，`netstat -an | grep tcp | grep 3129`可以查看端口运行的进程，但没有管理员权限无法得知确切进程。且不知道出于什么原因，即使该端口被占用或者是其它原因，网络仍然可以连接。

- 其它连接失败的解决方法
  - 将vpn从rules切换到global
  - 如果vscode拓展不能正常加载，在设置中搜索proxy并关闭它

# 9. 使用拓展

- 理论上来说，从此也可以使用git在远程管理代码
