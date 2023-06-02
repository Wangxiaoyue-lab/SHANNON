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

# 3. vpn(理论上可选)

- 如果要使用vpn

  If Host A itself uses some proxy say `10.140.78.130:8080` （just as the example）to connect to internet then also add that proxy to `/etc/squid/squid.conf` as follows:

  ```config
  refresh_pattern (Release|Packages(.gz)*)$ 0 20% 2880
  cache_peer 10.140.78.130 parent 8080 0 no-query default
  never_direct allow all
  ```

  这里我使用的是clash，静态ip，因此第二行为
   `cache_peer 127.0.0.1 parent 7890 0 no-query default`
- 如果不使用vpn
  我没有尝试过，因为我在关闭clash时出现了无法连接的问题

# 4. 修改服务器bashrc

Host B 上:将下面的语句加入bashrc并source。原教程中使用的是environment，没有root实现不了。

```
export http_proxy=http://127.0.0.1:3129
export https_proxy=http://127.0.0.1:3129
```

# 5. 启动本地squid
安装后有Squid Server Tray的快捷方式，双击启动

# 6. 服务器添加端口转发即可运行

- 在vscode中，在端口中加上3128即可
  或者，在ssh默认配置文件中，可以添加这一行以永久forward这个端口
  `RemoteForward 3129 localhost:3128`
  - 添加完后需要重启vscode
- 如果没有vscode
  `ssh -R 3129:localhost:3128 user@10.168.203.60`


# 7. 测试

```shell
wget www.google.com
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

# 8. 一些可能的debug办法

- 将vpn从rules切换到global
- 如果vscode拓展不能正常加载，在设置中搜索proxy并关闭它

