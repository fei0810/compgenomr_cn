---
title: '2020-07-18T00:00:00.000Z'
date: '2020-07-18 18:50'
---

# 2.2 从 R 起步

下载并安装 [R](http://cran.r-project.org/) 和 [RStudio](http://www.rstudio.com/) 。虽然 Rstudio 并非必须但它是一个很好的工具，如果你刚刚开始学习 R 非常建议你使用 Rstudio 进行学习，你需要特定的数据集来运行本文档中的代码。下载 data.zip 并将其解压到你选择的目录，文件夹名称应该是‘data’，你的 R 工作目录应该在 data 文件夹上一层级，也就是在你的 R 控制台中输入 `dir("data")` 时，应该能够看到数据文件夹的内容。

你可以通过 `setwd()` 命令更改工作目录，使用 `getwd()` 命令获取当前工作目录。在 RStudio 中也可以单击顶部菜单并通过可视化的操作来更改工作目录的位置。

> 译者注： 目前实际并没有 data.zip 这个数据，本书中使用到的数据可以通过 devtools::install\_github\("compgenomr/compGenomRData"\) 进行安装。 
>
> 同时我也把这个数据包上传到了网盘，你可以下载后在本地通过 install.packages\("~/Desktop/compGenomRData.tar.gz", repos=NULL,type="source"\) 进行安装 
>
> 网盘链接: [https://pan.baidu.com/s/1y8R4b1O1u6Q\_5GR1OHGtKQ](https://pan.baidu.com/s/1y8R4b1O1u6Q_5GR1OHGtKQ) 密码: `24mh`
>
> 备用链接：[https://kaopubear.cowtransfer.com/s/f872844c5b0346](https://kaopubear.cowtransfer.com/s/f872844c5b0346) 密码：`kaopubaer`

## 2.2.1 安装包

R 包可以理解为基础 R 的附加内容，可以帮你实现基础 R 中不直接支持的任务。正是通过这些扩展包才让 R 成为适合计算基因组学的工具。[Bioconductor](http://bioconductor.org/) 项目是计算生物学相关软件包的专用库，同时 R 的主包存储库 CRAN 也有计算生物学相关的包。除此以外，[R-Forge](http://bioconductor.org/)，[GitHub](http://bioconductor.org/) 和 [googlecode](http://bioconductor.org/) 也可能托管了部分 R 包。

你可以用 `install.packages()` 安装 CRAN 包\(需要说明， `#` 是 R 中的注释字符\)。

```r
# 从 CRAN 安装名为 "randomForests" 的 R 包
install.packages("randomForests")
```

你可以使用特定的安装方法来安装 bioconductor 包。

```r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("rtracklayer")
```

可以使用 devtools 的 `install_github()` 函数从 github 安装软件包。

```r
library(devtools)
install_github("hadley/stringr")
```

> 译者注：从 GitHub 或者 Bitbucket 等地方安装 R 包，还可以借助 [remote 包](https://remotes.r-lib.org/#:~:text=remotes%20Install%20R%20Packages%20from%20remote%20or%20local,lightweight%20replacement%20of%20the%20install_%2A%20functions%20in%20devtools.)进行安装。Remote 本身可以理解为 devtools 一系列 install\_\* 函数的精简版本。remote 同时支持下载 bioconductor 包。

安装软件包的另一种方法是从源代码进行本地安装。

```r
# 下载源文件
download.file("http://goo.gl/3pvHYI",
               destfile="methylKit_0.5.7.tar.gz")
# 从源文件安装包
install.packages("methylKit_0.5.7.tar.gz",
                 repos=NULL,type="source")
# 删除源文件
unlink("methylKit_0.5.7.tar.gz")
```

你还可以更新 CRAN 和 Bioconductor 包。

```r
# 升级 CRAN 包
update.packages()
# 升级 bioconductor 包
BiocManager::install(update = T)
```

## 2.2.2 在自定义位置安装包

如果你在服务器或集群上使用 R，那就不太可能拥有管理员权限来安装包。在这种情况下可以通过告诉 R 在哪里寻找额外的包来自定义位置。

打开你家目录中的 renvironon 文件，并添加以下行:

```text
R_LIBS=~/Rlibs
```

这句命令将告诉 R 家目录的 'Rlibs' 目录是查找包和安装包的第一位置选择，接下来你应该去创建那个目录然后启动一个新的 R 会话并开始安装包。这之后的 R 包将被安装到你具有读写访问权限的本地目录中。

> 译者注：关于 R 的相关配置和在 macOS 中的安装，可以参考译者早前写过的两篇文章 [R 安装升级后的若干规定动作](https://kaopubear.top/blog/2018-07-09-chineseuser/) 和 [macOS 10.15 安装 R 包](https://kaopubear.top/blog/2019-10-29-macos15user/) 。

## 2.2.3 获取有关函数和包的帮助信息

你可以通过 `help()` 和 `help.search()` 函数获得有关函数的帮助文档。也可以用 `ls()` 函数列出一个包中的函数

```r
library(MASS)
ls("package:MASS") # functions in the package
ls() # objects in your R enviroment
# get help on hist() function
?hist
help("hist")
# search the word "hist" in help pages
help.search("hist")
??hist
```

### 2.2.3.1 需要更多帮助？

此外，你还可以检查包的 vignette 以获得帮助和对函数更深入实际的理解。所有 Bionconductor 包都有 vignette 来引导你完成示例分析。学会搜索也总是有帮助的，有许多博客和网页上都有关于 R 的帖子，Stackoverflow 和 R-blogger 通常是优秀和可靠的信息来源。

