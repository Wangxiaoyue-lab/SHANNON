# jupyter

参考资料：
[远程连接集群（Slurm）上的 jupyter - 知乎](https://zhuanlan.zhihu.com/p/451185184)

[reticulate：在 R 中使用 Python - 知乎](https://zhuanlan.zhihu.com/p/164507492)

# srun 使用 jupyter

1. 背景：学校集群是 Slurm 调度系统，用户需要从自己的私人电脑（windows 本地）先连接到跳板机（中间登录节点），再申请计算节点，在计算节点上进行操作。

首先还是下载并且配置计算节点上的 jupyter，这里与其他配置方法相同，不再赘述。

2. 配置好后在节点(compute60)上启动 jupyter
   `srun --pty --mem=100G -c 10 jupyter lab --no-browser --port=8000 --ip=localhost`

3. 建立隧道

- 在没有 vscode 的情况下，本机 shell 建立隧道。

  ```
  ssh -t -t  user48@2.1.1.5 -L 8014:localhost:8014 ssh 1.1.1.4 -L 8014:127.0.0.1:8014
  # 这里user48是中间登录节点的用户名，2.1.1.5是中间登录节点的IP，1.1.1.4是计算节点的IP，8014是port号可以换成你自己喜欢的号码。
  # 因此我的是
  ssh -t -t  luoliheng@10.168.203.60 -L 8000:localhost:8000 ssh 10.168.203.12 -L 8000:127.0.0.1:8000
  ```

  这段代码使用 ssh 命令建立了两个端口转发。
  第一条命令使用-t -t 选项强制分配伪终端，然后使用-L 选项将本地计算机的端口 8000 转发到远程计算机 10.168.203.60 的端口 8000。
  第二条命令在远程计算机 10.168.203.60 上运行，它将远程计算机的端口 8000 转发到另一个远程计算机 10.168.203.12 的端口 8000。因为此时我是在计算节点运行的。

- 在有 vscode 的情况下呢？
  第一条命令 vscode 会自动转发
  第二条命令，则直接在 60 节点里运行 `ssh 10.168.203.12 -L 8000:127.0.0.1:8000`即可

4. 建立隧道完成后，在本地浏览器输入如下代码，即可成功访问。

   ```
   https://127.0.0.1:8000
   ```

5. 简便做法
   每次要 squeue 获知计算节点，然后转发端口太麻烦，因此我已经写好了脚本，路径在`/public/home/luoliheng/share/scripts/port_forward.sh`

   它的功能是自动获得计算节点 ip 并转发端口。

   copy 以后，既可以在命令行中
   `alias port_forward="bash /public/home/luoliheng/share/scripts/port_forward.sh"`
   也可以删除其中的#号，它在 bash 时就会自动 alias。
   然后重启命令行就可以使用`port_forward [port]`命令。如果不填 port 默认 8000。也可以直接在文件里修改默认值。

- 附：协和高算计算节点对应的 IP:
  ```
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

# notebook 文件转换

使用命令行工具 `jupyter nbconvert` 将 Jupyter Notebook 转换为 Python 脚本。例如，要将名为 `notebook.ipynb` 的 Jupyter Notebook 转换为 Python 脚本，可以在命令行中运行以下命令：

```
jupyter nbconvert --to script notebook.ipynb
```

复制

这将在同一目录下生成一个名为 `notebook.py` 的 Python 脚本文件。

对于 R 脚本，您可以使用上面提到的 `rmarkdown` 包中的 `convert_ipynb` 函数和 `knitr::purl()` 函数。您可以在 R 命令行中运行这些函数，也可以编写一个 R 脚本来执行这些操作。

您可以使用以下 R 脚本将 Jupyter Notebook 转换为 R 脚本。该脚本接受一个命令行参数，即要转换的 Jupyter Notebook 文件的路径。它将在同一目录下生成一个与 Jupyter Notebook 文件同名的 R 脚本文件。

```r
#!/usr/bin/env Rscript

# 检查命令行参数
if (length(commandArgs(trailingOnly = TRUE)) < 1) {
  stop("请提供要转换的 Jupyter Notebook 文件的路径", call. = FALSE)
}

# 获取 Jupyter Notebook 文件的路径
notebook_file <- commandArgs(trailingOnly = TRUE)[1]

# 加载必要的包
library(rmarkdown)
library(knitr)

# 将 Jupyter Notebook 转换为 R Markdown 文件
rmd_file <- sub("\\.ipynb$", ".Rmd", notebook_file)
convert_ipynb(notebook_file, output = rmd_file)

# 将 R Markdown 文件转换为 R 脚本
r_script_file <- sub("\\.Rmd$", ".R", rmd_file)
purl(rmd_file, output = r_script_file)

# 删除中间文件
file.remove(rmd_file)

cat(paste("已将", notebook_file, "转换为", r_script_file, "\n"))
```

复制

您可以将上述代码保存为一个文件，例如 `convert_notebook_to_r_script.R`，然后在命令行中运行以下命令来使用它：

```
Rscript convert_notebook_to_r_script.R notebook.ipynb
```

复制

其中 `notebook.ipynb` 是要转换的 Jupyter Notebook 文件的路径。
