下载步骤：

1. 打开下载链接，下载 linux 客户端并传递到远程
2. 创建文件夹，确定路径。如客户端路径为

- `/public/home/luoliheng/packages/linuxnd/lnd`
  要下载的文件路径为
- `/public/home/luoliheng/perturb/raw_data_230609`

3. 准备网络
   _注意:下载大型数据可能会花费数小时，请注意开始下载的时间_
   _或者采用本地转接的方式下载(如在 clash 中直接设置为 direct 模式)，有时本地转接甚至比在服务器上下载还要快，平均可达到 15M/s_

- 如果不能设置为 direct，附取消代理的方式：

  ```bash
  # 注意，下载大型数据时不要使用国外代理，浪费流量且慢
  unset http_proxy
  unset https_proxy
  # 输入以上命令即可


  # 请注意，取消设置这些变量只会禁用当前会话的代理
  # 如果想永久禁用代理，则需要从shell启动文件(例如：.bashrc，.bash_profile)
  # 或系统范围的配置文件(例如：/etc/environment)中删除
  ```

4. 登陆并全部下载

```bash
# 登陆
./lnd login -u X101SC23031823-Z01-J004 -p ******
# 下载
./lnd cp -d oss://CP2023030700120 /public/home/luoliheng/perturb/raw_data_230609



```
