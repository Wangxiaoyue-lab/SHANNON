# change your group
```
# It takes a lot of time
chgrp -R wanglab ~
```



# how to transfer data in differernt servers?

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

