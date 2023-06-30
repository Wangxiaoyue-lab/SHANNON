# srun 使用 jupyter

- 简便做法:

> 每次要 squeue 获知计算节点，然后转发端口太麻烦，因此我已经写好了脚本，路径在 `/public/home/luoliheng/share/scripts/forward_port.sh`
> 它的功能是自动获得计算节点 ip 并转发端口。
> 可以在 bashrc 中添加:
>
> `alias port_forward="bash /public/home/luoliheng/share/scripts/forward_port.sh"`
>
> 最后 ` source ~/.bashrc`就可以随时随地使用 `port_forward -r[row] -p[port]`命令来进行端口转发了。
>
> - 如果不填 port 默认 8000。也可以直接在文件的第一行里修改默认值。
> - -r[row]是什么意思？squeue 后会获得正在运行的计算节点，有可能有多个脚本同时运行，此时应该指定需要对应哪个任务所在行的节点，默认是第一个任务对应节点。

- 有没有可能使 jupyter 运行之后自动转发呢？有可能，只是我目前不知道怎么做。

# srun 使用 X11 图形界面

`alias x_forward="bash /public/home/luoliheng/share/scripts/ssh_x.sh"`即可。
也可以指定任务，第一个参数默认是第一行的任务。

# 附：协和高算计算节点对应的 IP:

```shell
## Management Ethernet Network ##
10.168.203.193        comput1        node1
10.168.203.2        comput2        node2
10.168.203.3        comput3        node3
10.168.203.4        comput4        node4
10.168.203.5        comput5        node5
10.168.203.[6-64]   comput[6-64]   node[6-64]
10.168.203.181      gpu1           node181
10.168.203.184      fat1           node184
10.168.203.188      admin1         node188
10.168.203.190      login1         node190
```
