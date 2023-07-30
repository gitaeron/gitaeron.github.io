---
title: "Conda环境中 Tensorflow 与 CPU 和 GPU 的问题"
date: 2023-01-04T14:16:00+08:00
author: "澜长生"

readingTime: 15
categories: ["技术"]
tags: ["Tensorflow", "Artificial-Intelligence"]
---

# 首先

这两天一直在解决 conda(我用的是miniconda) 环境中 `Tensorflow` 使用 CPU 和 GPU 的问题, 主要是关于 GPU.

这一篇文章想对这些问题总结, 攥写一篇心得.

放心, 不是网络上各种文章的copy, 我反而希望你是在尝试了主流文章后无法解决的情况下看到这篇文章.

当然有引用的内容会注明来源(没什么引用,纯个人经验), 但是我相信这篇文章可以解决你所碰到的相同的问题.

---

{{<toc>}}

<!--more-->

---

## 1. conda环境

首先我们通过以下命令建立了一个 conda 虚拟环境

- `conda create -n ENV_NAME python=3.8`

然后我相信你根据 Tensorflow 官网给出的安装指导, 通过以下命令安装了 Tensorflow

- `pip install tensorflow`

然后我们就开始了折腾之旅.

## 2. 宿主机外部 CUDA & cudnn

首先, 如果你根据 google 搜索到的大部分文章, 都会告诉你下载并安装 CUDA & cudnn, 就不在此赘述, 因为本篇文章的核心在下面.

这里的**宿主机外部** CUDA & cudnn 指的是你安装这些工具并配置好 windows 环境变量以后, 通过

- `nvidia-smi`

这条指令, 你可以看到你的显卡了, 并且把 cudnn 正确的移动到了对应的文件夹.

其实**虚拟环境外**的 CUDA & cudnn 对于你使用 conda 的虚拟环境没有太大的帮助, 这句话代表了本文通篇的观点, 并作为本文的解决方案的总结.

## 3. conda虚拟环境内部 CUDA & cudnn

如果你在对应的虚拟环境中根据 Tensorflow 官网的指导使用 `pip install tensorflow` 安装了 Tensorflow,

那么在此恭喜你, 你可以重新创建一个虚拟环境了, 这个环境留作实验环境.

根据这个标题, 什么叫 `conda虚拟环境内部 CUDA & cudnn` 呢? 如你所阅读理解到的内容, 我们需要在虚拟环境内部安装 CUDA & cudnn.

> 特别指出: conda 会读取 windows 的环境变量这点是没错, 
> 
> 但是这并不代表你可以使用特定虚拟环境中的 Tensorflow 通过读取 windows环境变量使用**外部的** CUDA & cudnn

所以, 接下来你需要先打开这个链接 => [Tensorflow官网 - 经过测试的构建配置 - GPU](https://www.tensorflow.org/install/source_windows?hl=zh-cn#gpu) 

并注意 `最新的, 经过官方适配的` Tensorflow版本 与 CUDA & cudnn 对应版本. 

- 2023-01-04 本文完成书写时, GPU的适配版本还是 Tensorflow 2.6

| 版本 | Python 版本 | 编译器构建工具 | cuDNN | CUDA |
| :--: | :--: | :--: | :--: | :--: |
| tensorflow_gpu-2.6.0	| 3.6-3.9 | MSVC 2019 Bazel 3.7.2 | 8.1 | 11.2 |

---

## 4. 定位问题并解决问题

所以, 对于上一个标题所说的你应该把使用 `pip install tensorflow` 所安装的 tensorflow 虚拟环境留作实验, 

是因为你安装了最新的 tensorflow版本, 并且没人经过实验后, 告诉你应该安装哪一个版本的 CUDA & cudnn 来适配你所安装的 tensorflow版本的. 

并且根据本文所提供的解决方案, 你能找得到的 conda源 或 第三方源 中 CUDA & cudnn 的版本也止步于 tensorflow_gpu 所适配的 CUDA => 11.2 & cudnn => 8.1

所以你现在知道问题出在哪里了对不对, 那我相信你已经知道怎么解决了.

> 如果你根据我在上述文本中提供的线索无法定位问题, 那么以下是我对你所碰到的环境问题的总结:
>
> 1. conda 虚拟环境中安装的 tensorflow 无法使用外部环境中的 CUDA & cudnn
> 2. conda源或第三方源的 CUDA & cudnn 版本止步于 tensorflow_gpu 2.6 版本的适配版本

所以你解决了其中任意一个问题, 对于你所碰到的环境问题都是对症下药的.

那么本文根据 `问题1: conda 虚拟环境中安装的 tensorflow 无法使用外部环境中的 CUDA & cudnn` 来解决环境问题.

所以你现在应该根据我提供的以下命令新建一个正确的虚拟环境:

- `conda create -n ENV_NAME python=3.8` 

此处python版本随意, 官网给出的python版本范围即可, 不过我们都知道越旧越好 : ) 

- `pip install tensorflow==2.6.0`

- `conda install cudatoolkit=11.3` 

- `conda install cudnn=8.1`

> Tips: 此处的 CUDA 版本之所以是 11.3
>
> 原因其一是因为经过笔者的使用, 是 CUDA 版本11.3是可以适配 tensorflow_gpu 2.6.0 版本的; 
>
> 其二是因为 conda官方源只有 11.3, 经过笔者的努力, 是有一个 CUDA 版本为 11.2 的第三方源, 但是挂了代理下载也十分缓慢, **若你跟以往一样吹毛求疵**, 那么请自行 google 搜索这个第三方源并下载它. 

(我知道, 杠精就是你 =,= , 哪怕学会了魔法依旧没办法在如此广大的世界, 如此浩瀚的信息海洋中突破自我, 变成更好的自己, 真是可怜; 不过我也如此, 我也不过突破了当前的阶段达到下一个阶段而沉迷于其中无法自拔, 毕竟突破认知非朝夕之事. 愿你好自为之, 唯一的 key point 就是吾日三省吾身, 时常自省.)

经过以上命令所安装的虚拟环境, 我想你是可以通过 Run 一下相关代码发现你终于可以跑起来啦! (但是因为你的显存不够又炸了.jpg)

---

## Tips: 若你只有5分钟, 那么止步于此吧, 以下是进阶内容

---

## 5. 你会碰到一个问题

问题如下, 不过我相信你会通过 google 搜索问题并得到相关的解决方案, 所以这里只是顺带一提.

[ImportError: cannot import name "dtensor" from "tensorflow.compat.v2.experimental"](https://stackoverflow.com/questions/72255562/cannot-import-name-dtensor-from-tensorflow-compat-v2-experimental)

由于 `pip install tensorflow==2.6.0` 时, 安装的 `keras` 版本是 2.11.0, 导致了 incompatibility.

解决办法就是

- upgrade `tensorflow==2.8`

- downgrade `keras==2.6.*` 

然而前面已经描述过相关内容, 所以这里应该怎么选择解决方案各位是都清楚的.

- `Tensorflow-GPU` V2.6 => CUDA V11.2 CUDNN V8.1

| 版本 | Python 版本 | 编译器构建工具 | cuDNN | CUDA |
| :--: | :--: | :--: | :--: | :--: |
| tensorflow_gpu-2.6.0	| 3.6-3.9 | MSVC 2019 Bazel 3.7.2 | 8.1 | 11.2 |

</br>

---

## 6. pip install tensorflow 

首先我的建议是:

> 使用 `pip install tensorflow` 来安装具有 CPU & GPU 的版本
>
> 同时这也是官方给出的办法

而不是使用以下的办法:

- `conda install tensorflow-gpu`

- `pip install tensorflow-gpu`

---

## 7. 如果你想指定 CPU/GPU

对于这个需求, 笔者的解决方案是安装了两个环境

> 一个是只能使用 CPU 的 Tensorflow 最新版, 因为它不含 CUDA & cudnn , 使用以下命令安装 Tensorflow

- `pip install tensorflow`

> 还有一个就是使用 GPU/CPU 的 Tensorflow_gpu 2.6.0, 包含了 CUDA & cudnn, 使用以下命令安装.

- `pip install tensorflow=2.6.0`

我记得应该是可以 google 到相关需求的解决办法的, 但是我使用了更简单粗暴的方法....

---

## 8. 经过测试的构建配置

以下列出完整的配队列表 (截止至 2023-01-04, 并给你 [官方的链接](https://www.tensorflow.org/install/source_windows?hl=zh-cn#gpu) 让你收藏备用.

> CPU

| 版本 | Python 版本 | 编译器 | 构建工具 |
| :--: | :--: | :--: | :--: | 
| tensorflow-2.6.0	 | 3.6-3.9 | MSVC 2019 Bazel 3.7.2 |
| tensorflow-2.5.0	 | 3.6-3.9 | MSVC 2019 Bazel 3.7.2 |
| tensorflow-2.4.0	 | 3.6-3.8 | MSVC 2019 Bazel 3.1.0 |
| tensorflow-2.3.0	 | 3.5-3.8 | MSVC 2019 Bazel 3.1.0 |
| tensorflow-2.2.0	 | 3.5-3.8 | MSVC 2019 Bazel 2.0.0 |
| tensorflow-2.1.0	 | 3.5-3.7 | MSVC 2019 Bazel 0.27.1-0.29.1 |
| tensorflow-2.0.0	 | 3.5-3.7 | MSVC 2017 Bazel 0.26.1 |
| tensorflow-1.15.0 | 3.5-3.7 | MSVC 2017 Bazel 0.26.1 |
| tensorflow-1.14.0 | 3.5-3.7 | MSVC 2017 Bazel 0.24.1-0.25.2 |
| tensorflow-1.13.0 | 3.5-3.7 | MSVC 2015 update 3 Bazel 0.19.0-0.21.0 |
| tensorflow-1.12.0 | 3.5-3.6 | MSVC 2015 update 3 Bazel 0.15.0 |
| tensorflow-1.11.0 | 3.5-3.6 | MSVC 2015 update 3 Bazel 0.15.0 |
| tensorflow-1.10.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.9.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.8.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.7.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.6.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.5.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.4.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.3.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.2.0 | 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.1.0 | 3.5 | MSVC 2015 update 3 Cmake v3.6.3 |
| tensorflow-1.0.0 | 3.5 | MSVC 2015 update 3 Cmake v3.6.3 |

---

> GPU

| 版本 | Python 版本 | 编译器构建工具 | cuDNN | CUDA |
| :--: | :--: | :--: | :--: | :--: |
| tensorflow_gpu-2.6.0	| 3.6-3.9 | MSVC 2019 Bazel 3.7.2 | 8.1	| 11.2 |
| tensorflow_gpu-2.5.0	| 3.6-3.9 | MSVC 2019 Bazel 3.7.2 | 8.1	| 11.2 |
| tensorflow_gpu-2.4.0	| 3.6-3.8 | MSVC 2019 Bazel 3.1.0 | 8.0	| 11.0 |
| tensorflow_gpu-2.3.0	| 3.5-3.8 | MSVC 2019 Bazel 3.1.0 | 7.6	| 10.1 |
| tensorflow_gpu-2.2.0	| 3.5-3.8 | MSVC 2019 Bazel 2.0.0 | 7.6	| 10.1 |
| tensorflow_gpu-2.1.0	| 3.5-3.7 | MSVC 2019 Bazel 0.27.1-0.29.1 |7.6 | 10.1 |
| tensorflow_gpu-2.0.0	| 3.5-3.7 | MSVC 2017 Bazel 0.26.1 | 7.4 | 10 |
| tensorflow_gpu-1.15.0	| 3.5-3.7 | MSVC 2017 Bazel 0.26.1 | 7.4 | 10 |
| tensorflow_gpu-1.14.0	| 3.5-3.7 | MSVC 2017 Bazel 0.24.1-0.25.2 | 7.4 | 10 |
| tensorflow_gpu-1.13.0	| 3.5-3.7 | MSVC 2015 update 3 Bazel 0.19.0-0.21.0 | 7.4 | 10 |
| tensorflow_gpu-1.12.0	| 3.5-3.6 | MSVC 2015 update 3 Bazel 0.15.0	7.2	9.0 |
| tensorflow_gpu-1.11.0	| 3.5-3.6 | MSVC 2015 update 3 Bazel 0.15.0 | 7 | 9 |
| tensorflow_gpu-1.10.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 7 | 9 |
| tensorflow_gpu-1.9.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 7 | 9 |
| tensorflow_gpu-1.8.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 7 | 9 |
| tensorflow_gpu-1.7.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 7 | 9 |
| tensorflow_gpu-1.6.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 7 | 9 |
| tensorflow_gpu-1.5.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 7 | 9 |
| tensorflow_gpu-1.4.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 6 | 8 |
| tensorflow_gpu-1.3.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 6 | 8 |
| tensorflow_gpu-1.2.0	| 3.5-3.6 | MSVC 2015 update 3 Cmake v3.6.3 | 5.1 | 8 |
| tensorflow_gpu-1.1.0	| 3.5 | MSVC 2015 update 3 Cmake v3.6.3 | 5.1 | 8 |
| tensorflow_gpu-1.0.0	| 3.5 | MSVC 2015 update 3 Cmake v3.6.3 | 5.1 | 8 |

</br>

---

## Refer

- [ImportError: cannot import name "dtensor" from "tensorflow.compat.v2.experimental"](https://stackoverflow.com/questions/72255562/cannot-import-name-dtensor-from-tensorflow-compat-v2-experimental)

- [经过测试的构建配置](https://www.tensorflow.org/install/source_windows?hl=zh-cn#gpu)



