---
title: PCM的A-law和μ-law的理解
images: /images/abstract_pic/
copyright: true
permalink: 1
date: 2019-08-09 17:17:28
tags: [PCM, A-law, μ-law]
categories: protocol
password:
---
**摘要** ：PCM(Pulse code modulation) 脉冲编码调制是一种常见的数字化量化模拟波形的方式，其中有两个常用的压缩算法，A-law 和 μ-law 算法。
<!-- more -->
# 简介
在语音编码的情况下，8kHz采样频率 & 13bits线性量化是准确产生全域语音信号的数字表示的最小需求，对许多的有线或者无线的传输系统来说，即便只是带宽，已经算是一个昂贵的方式 (expensive proposition)。为了解决这个问题，通常会使用压缩算法，即A-law，和 μ-law。这两种非线性压缩算法是为了将采样到 12 或者 13bits 的幅值数据转化为 8bits，其中 A-law 是欧洲公认的标准，μ-law 是美国日本公认的标准。
# μ-law
## 数学模型

![μ-law数学公式.png](https://i.loli.net/2019/08/12/54zV8uAkeNFIsSC.png)

注：sgn(x) 为阶跃函数，在此，x = 0 时 sgn(0) = 0；x > 0 时，sgn = 1；x < 0 时，sgn = -1；μ-law 可由以上数学公式定义，其中 μ = 255，x 为归一化的值，此函数的线性逼近曲线如下图。注意此图只显示了 x > = 0 的情况。

![μ-law线性逼近曲线.png](https://i.loli.net/2019/08/12/mM3FqXkHWURASbO.png)

## 压缩编码

![μ-law压缩编码.png](https://i.loli.net/2019/08/12/3LvFhZV4HCOpwxb.png)

μ-law 的具体编码方式如上表所示，注意符号位并未显示，在 μ-law 中，符号位 = 1 表示正，符号位 = 0 表示负，在 13 位数据输入后，舍掉前面的 0，以第一个 bit 为 1 处作为 chord，这个 bit 为 1 将会被编码成 3 bit 的二进制数，即 Compressed Code Word 里，符号位后面三位，随后的 4 个 bit abcd 保持不变，d 之后的位数被舍掉。你可能发现，在举出的编码方式中，没有输入数据 bit4 为 1，前面全为 0 的情况，这是因为我们只有 3 个 bit 来表示 chord，也就是说只能从 bit12 到 bit5，为了解决这个问题，μ-law 规定在压缩之前，在去掉符号位之后，在传入的数据上加上 33，这样就能够避免之前的情况，即便采样值为 0，传入的数据第 5 个 bit 仍然可以为 1。

另外注意，在传输之前，会再将压缩后的编码反转一次，因为通常来说，低振幅信号比高振幅信号多，反转使得传输线上正脉冲的密度增加，可以提高硬件的性能。

## 解码

解码操作本质上就是压缩编码的反向过程，只是之前舍掉的 abcd 后面的位数，由在 abcd 后面补充一个 1，也就是取区间的中间值来补充上。注意先反转回去。如下图所示，未显示符号位。

![μ-law解码.png](https://i.loli.net/2019/08/12/KdheiL9HM3rYxNo.png)

这种算法的动态范围也可以由以下的式子算出：

![μ-law动态范围.png](https://i.loli.net/2019/08/12/z3b69AuR1fhgxD8.png)

其中 8159 为可能的最大幅值，31 为到达第一个 chord 的最低幅值。

# A-law
## 数学模型

![A-law数学模型.png](https://i.loli.net/2019/08/12/nymIsrlKXTwkOSe.png)

A-law 可由以上函数方程表示，A = 87.7，x 为归一化的参数，sgn 为阶跃函数，此函数的线性逼近曲线如下图。注意此图只显示了x > = 0 的情况。

![A-law线性逼近曲线.png](https://i.loli.net/2019/08/12/ldcjTnkMuz7K8aO.png)


## 压缩编码

![A-law压缩编码.png](https://i.loli.net/2019/08/12/7FaPrLkZAEDfqgp.png)

编码表如上图所示，符号位未显示，编码方式与 μ-law 类似，不同的是符号位与 μ-law 相反，A-law 中，符号位为 1 表示负，符号位为 0 表示正，且 A-law 只能接受幅值为 12bit 的输入数据，这也使得其 chord 的定义有所不同。舍掉前面的 0，从 11-5bit 为 1，分别由三位的二进制数 6-5bit 编码，不同的是 '000' 的情况表示从 11bit 到 5bit 都为 0 的情况，abcd 取 4~1 位，0bit 舍掉。

另外，与 μ-law 一样，在传输之前仍然要进行一次反转，来提高硬件性能。


## 解码

解码的本质就是颠倒编码的步骤，解码表如下图所示。也是与 μ-law 类似，在之后补一个 1，也就是取中间值。注意先反转回去。

![A-law解码.png](https://i.loli.net/2019/08/12/zudD19WQvwIJEH8.png)

这种算法的动态范围也可以由以下的式子算出：

![A-law动态范围.png](https://i.loli.net/2019/08/12/jfy81pVcDguraew.png)

其中 4096 为可能的最大幅值，15 为到达第一个 chord 的最低幅值。