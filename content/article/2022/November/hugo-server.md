---
title: "运行 `hugo server` 期间出现的预览问题"
date: 2022-11-27T16:44:00+08:00
author: "澜长生"

readingTime: 5
categories: ["技术"]
tags: ["Hugo", "Bilberry Theme"]
---


# 发生了什么

- 在运行 `hugo server` 期间, 发现了部分操作导致出现404页面.

在运行 `hugo server` 时, 当你修改了一篇文章的 `categories` 或者 `tags` 时, 你通过任何途径点击修改后的 `categories` 和 `tags` 都会出现访问的页面404.

# 多图警告

<!--more-->


<br/>

---

<br/>


# 过程描述

例如:

1. 本文的 `categories` 和 `tags` 是这样的:

- categories: ['tech']
- tags: ['hugo', 'issue', 'experience']

![1](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/1.png)

<br/>

2. 当运行 `hugo server` 以后, 可以直接通过点击 `categories` 或 `tags` 跳转到相应页面.

![2](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/2.png)

<br/>

3. 当你在编辑器中更改文章的 `categories` 或 `tags` 之后
   
![3](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/3.png)

<br/>

![4](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/4.png)

<br/>

4. 结果出现404

![5](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/5.png)

<br/>

5. 这种情况包括但不限于在 **运行 `hugo server` 时**, 对 `gategories` , `tags` , `author` etc.. 进行更改.

![6](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/6.png)

<br/>
   
6. 这种情况对于 **运行 `hugo server` 后** 新添加的文章同样有效.

![7](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/7.png)


<br/>

---

<br/>

# 解决办法

很简单的 `Ctrl` + `C`

然后运行 `hugo server`

![8](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/8.png)

<br/>

---

<br/>

# 结论

我也不知道为啥, 但是就是这样.

毕竟我甚至都不是前端工程师或全栈工程师... &emsp; :(
    
这篇文章算是一个小Tips吧...

