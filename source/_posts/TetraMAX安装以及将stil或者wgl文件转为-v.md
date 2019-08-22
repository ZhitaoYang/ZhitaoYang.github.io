---
title: TetraMAX安装以及将.stil或者.wgl文件转为.v
images: /images/abstract_pic/
copyright: true
permalink: 1
date: 2019-08-21 13:41:06
tags: [TetraMAX]
categories: [TetraMAX]
password:
---
**摘要** ：近来实习的时候，遇到需要将 **.stil** 或者 **.wgl** 文件转化为 **.v** 的文件，作为 Test bench 来对设计进行测试，在此记录下所用的方法。
<!-- more -->
# 简介

TetraMAX 是一个高速，高性能的自动测试激励产生工具 （ATPG automatic test patterngeneration)，它能使用最少的测试向量来产生最大测试覆盖的测试激励。TetraMAX 是 synopsys公司的测试工具，它可以和很多EDA工具一起来完成测试工作。

# TetraMAX 安装
## 安装包下载
TetraMAX 的安装包可以在各个地方都能找到，我这里也提供一个百度云的链接，版本是2015.06的。  
[TetraMAX 15.06](https://pan.baidu.com/s/1frvAJ8NoCmsRypiKer6NYg)  
提取码：qnxx  
## 安装包导入
因为我是在 windows 下下载的压缩包，而我需要将这个软件装到我们的linux服务器上，首先要将此文件从 windows 传入 linux。我用的是 secureCRT 这个软件的 sftp 来传输文件，当然传输文件的方法很多，只要百度一下就行。  
在 secureCRT 连接到服务器之后，点击 file --> Connect SFTP Session 进入 sftp 的命令行，此时用 `lcd` 来进入本地需要传输文件的路径，`l` 表示 local，可用类似的命令 `lcd`，`lls` 等来查看本地的目录，不加 `l` 表示服务器上的目录。 将本地路径调为需要传输的文件所在目录，将服务器路径调为需要放置文件的目录，然后使用 `put yourfile` 来传输文件。压缩包传入之后，自然是用 linux 下的解压缩命令来解压，我的是 `.tar.gz` 文件，使用命令  
`tar -zxvf yourfile.tar.gz -C /tools/Synopsys/TetraMAX`。  
## 环境配置
在用户目录 `~` 下的 **.bashrc** 文件里，加入以下代码：
```
#TetraMAX
export PATH=$PATH:/home/users/project/tools/Synopsys/TetraMAX/k-2015.06/bin
export TMAX_HOME=/home/users/project/tools/Synopsys/TetraMAX/k-2015.06
alias tmax = "tmax -gui"
```
注意里面的路径是你的文件夹所在路径，然后执行 `source ~/.bashrc` 来使其立刻生效。

# 文件转换
然后便可以建一个文件夹，打开终端，输入 `tmax` 来打开 TetraMAX 的图像化界面了。打开图行化界面之后，输入命令 `stil2verilog STIL_pattern_file_name Verilog_Testbench_file_name` 即可，也可以用图像化界面 **Patterns --> Writte Testbench** 来选择要转化的文件。转化之后，会出来一个 **.dat** 和一个 **.v** 文件。
转化出来的 **.v** 文件就可以直接作为 Test Bench 来做测试了。

