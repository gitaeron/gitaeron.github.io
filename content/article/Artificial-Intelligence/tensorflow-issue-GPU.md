---
title: 'Tensorflow 与 CPU 和 GPU 问题'
date: 2022-12-15T14:16:00+08:00
author: "澜冬雪"
draft: true

readingTime: 10
categories: ['技术']
tags: ['Tensorflow', 'Artificial-Intelligence']

series: "人工智能系列"
---

# 首先

这两天一直在解决conda环境中 `Tensorflow` 使用 CPU 和 GPU 的问题, 主要是关于 GPU.

我用的是miniconda(其实都一样)

这一篇文章想对这些问题总结, 攥写一篇心得.

放心, 不是网络上各种文章的copy, 当然有引用的内容会注明来源, 但是我相信这篇文章可以解决你的大部分问题.

{{<toc>}}

<!--more-->

---

## 1. conda环境

## 2. 宿主机外部 CUDA & cudnn

## 3. conda虚拟环境内部 CUDA & cudnn

## 4. pip install tensorflow 

首先我的建议是:

> 使用 `pip install tensorflow` 来安装具有 CPU & GPU 的版本
>
> 同时这也是官方给出的办法

而不是使用以下的办法:

- `conda install tensorflow-gpu`

- `pip install tensorflow-gpu`



[ImportError: cannot import name 'dtensor' from 'tensorflow.compat.v2.experimental'](https://stackoverflow.com/questions/72255562/cannot-import-name-dtensor-from-tensorflow-compat-v2-experimental)


由于 `pip install tensorflow==2.6.0` 时, 安装的 `keras` 版本是 2.11.0, 导致了 incompatibility.

解决办法就是

- upgrade `tensorflow==2.8`

- downgrade `keras==2.6.*` 

因为前面已经说明了背景, 官方也给出 `Tensorflow-GPU` 只适配到

- `Tensorflow-GPU` V2.6 => CUDA V11.2 CUDNN V8.1

并且根据我的conda虚拟环境, `conda install cudnn` & `conda install cudatoolkit` 的版本是

- cudnn v8.2.1
- cudatoolkit v11.3.1

<!-- [tensorflow.python.framework.errors_impl.AlreadyExistsError: Another metric with the same name already exists.](https://stackoverflow.com/questions/58012741/error-importing-tensorflow-alreadyexistserror-another-metric-with-the-same-nam) -->

## 5. Tensorflow & CUDA & cudnn 版本号配对

## Refer

> To be continue ~


