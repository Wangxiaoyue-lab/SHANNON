# change your group
```
# It takes a lot of time
chgrp -R wanglab ~
```



# how to transfer data in differernt servers?
主要内容：打包环境并scp到另外的服务器
```shell
#1 打包BE环境
conda activate BE
conda pack -n BE -o BE_envs.tar.gz

#2 打包tf2环境
conda activate tf2
conda env export > tf2_env.yaml
pip freeze > requirements.txt 

# 改变权限
chmod 755 /home/gjj/BE_envs.tar.gz /home/gjj/requirements.txt /home/gjj/tf2_env.yaml

# 连接自己的ssh
cp /home/gjj/BE_envs.tar.gz /home/gjj/requirements.txt /home/gjj/tf2_env.yaml /home/llh/

#3 scp到生信中心
scp -oHostKeyAlgorithms=+ssh-rsa -P 22 BE_envs.tar.gz requirements.txt tf2_env.yaml luoliheng@222.28.163.113:/data/luoliheng


# The authenticity of host '222.28.163.113 (222.28.163.113)' can't be established.
# RSA key fingerprint is SHA256:hE3KUzLMPt63iSrDMcnNyInYfuGvu2es1XMyZV2WKWs.
# This key is not known by any other names
# Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
# Warning: Permanently added '222.28.163.113' (RSA) to the list of known hosts.
# (luoliheng@222.28.163.113) Password:


#5 在转化楼scp

date 
Mon May 15 23:08:34 CST 2023

scp -oHostKeyAlgorithms=+ssh-rsa -P 22 luoliheng@222.28.163.113:/data/luoliheng/BE_envs.tar.gz /data/luoliheng/requirements.txt /data/luoliheng/tf2_env.yaml /public/home/luoliheng/be

#5 结束

# 对于BE
cd /public/home/luoliheng/miniconda3/envs
mkdir BE
cd BE
tar -zxf /public/home/luoliheng/be/BE_envs.tar.gz

# 对于tf2
mamba env create -f /public/home/luoliheng/be/tf2_env.yaml
# 清华云已经弃用，因此无法使用, 从yaml文件中删除后替换成自己的镜像
# 使用mamba创建环境时，pip包会自动使用pip安装
# requirements.txt 同时存储某些未被conda识别的包以及本地包

```

# 如何使用服务器连接本地网络或者VPN
参考资料：
1. [ssh tunneling - Give server access to Internet, via client connecting by SSH - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/116191/give-server-access-to-internet-via-client-connecting-by-ssh)
2. [在Windows机器搭建squid代理\_squid windows\_(～￣▽￣)～凤凰涅槃的博客-CSDN博客](https://blog.csdn.net/qq_42704442/article/details/127746279)

Host A:本地windows
Host B:远程节点

首先，在参考资料2中下载squid并使其运行
接着，根据参考资料1，
Comment the `http_access deny all` then add `http_access allow all` in /etc/squid/squid.conf

If Host A itself uses some proxy say `10.140.78.130:8080` to connect to internet then also add that proxy to /etc/squid/squid.conf as follows:
```
refresh_pattern (Release|Packages(.gz)*)$ 0 20% 2880
cache_peer 10.140.78.130 parent 8080 0 no-query default
never_direct allow all
```
这里我使用的是clash，静态ip`cache_peer 127.0.0.1 parent 7890 0 no-query default`


Host B 上:将下面的语句加入bashrc并source
```
export http_proxy=http://127.0.0.1:3129
export https_proxy=http://127.0.0.1:3129
```

最后在Host A：windows的终端中，注意，命令完成后不要关闭终端
`ssh -R 3129:localhost:3128 luoliheng@10.168.203.60`