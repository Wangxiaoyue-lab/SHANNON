# 如何使用服务器连接本地网络或者VPN

参考资料：

1. [ssh tunneling - Give server access to Internet, via client connecting by SSH - Unix &amp; Linux Stack Exchange](https://unix.stackexchange.com/questions/116191/give-server-access-to-internet-via-client-connecting-by-ssh)
2. [在Windows机器搭建squid代理\_squid windows\_(～￣▽￣)～凤凰涅槃的博客-CSDN博客](https://blog.csdn.net/qq_42704442/article/details/127746279)

Host A:本地windows
Host B:远程节点

a. 首先，在参考资料2中下载squid并使其运行
b. 接着，根据参考资料1，
Comment the `http_access deny all` then add `http_access allow all` in /etc/squid/squid.conf

- 如果要使用vpn
  If Host A itself uses some proxy say `10.140.78.130:8080` to connect to internet then also add that proxy to /etc/squid/squid.conf as follows:

  ```
  refresh_pattern (Release|Packages(.gz)*)$ 0 20% 2880
  cache_peer 10.140.78.130 parent 8080 0 no-query default
  never_direct allow all
  ```

  这里我使用的是clash，静态ip `cache_peer 127.0.0.1 parent 7890 0 no-query default`
- 如果不使用vpn
  我没有尝试过，因为我在关闭clash时出现了无法连接的问题

c. Host B 上:将下面的语句加入bashrc并source。原教程中使用的是environment，没有root实现不了。

```
export http_proxy=http://127.0.0.1:3129
export https_proxy=http://127.0.0.1:3129
```

d. 最后在Host A：windows
注意，命令完成后不要关闭终端。此外，在使用之前，必须forward这个端口。

- 在vscode中，在端口中加上3128即可
  或者，在ssh默认配置文件中，可以添加这一行以永久forward这个端口
  `LocalForward 3128 localhost:3129`
- 如果没有vscode
  `ssh -L 3128:localhost:3129 user@remote_host`
  其中，-L表示本地转发，3128是本地端口，localhost:3129是远程主机和端口。这个命令将会把本地的3128端口转发到远程主机的3129端口。
  最后，在终端中使用 `ssh -R 3129:localhost:3128 luoliheng@10.168.203.60`，即可连接到本地网络。
