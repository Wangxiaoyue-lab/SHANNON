# jupyter

[远程连接集群（Slurm）上的 jupyter - 知乎](https://zhuanlan.zhihu.com/p/451185184)

[reticulate：在 R 中使用 Python - 知乎](https://zhuanlan.zhihu.com/p/164507492)

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
