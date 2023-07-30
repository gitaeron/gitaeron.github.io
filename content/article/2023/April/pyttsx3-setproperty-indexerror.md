---
title: "pyttsx3 setProperty IndexError"
date: 2023-04-18T21:55:00+08:00
author: "澜长生"

readingtime: 1
categories: ["技术"]
tags: ['pyttsx3']
---

# IndexError with setProperty('voice', value)

{{<toc>}}

<!--more-->

---

## 为什么会发生这个错误

因为本质上, pyttsx的实例对象属性 "id" 就是在注册表中语音的语音包序列, 而当你使用类方法时,

[pyttsx3_obj.setProperty('voice', value) ](https://pyttsx3.readthedocs.io/en/latest/engine.html#:~:text=setproperty(name%2C%20value)%20%E2%86%92%20none "setProperty(name, value) → None")

通过 name=voice 指定某一语音包,  value参数传入的是 pyttsx3.voice 这类属性中获取系统语音包后生成的序列id, 通过 pyttsx3_obj.id来调用, 所以这儿的错误类行为 IndexError, 也即索引越界.

说来说去其实就是注册表中的语音包缺少了, 所以你索引越界了.

---

## 如何解决

其实很简单, pyttsx3 中读取的语音包注册表路径是这个:

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Speech\Voices\Tokens\`

你在搜索这个错误的时候, 也许有些文章提到了这个路径 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Speech_OneCore\Voices\Tokens` 但是你不用去管它.

按下 `win` + `R` 快捷键, 输入 `regedit` 打开注册表, 以上面提到的路径打开, 你就会发现已下载的语音包数量, 索引是从0开始, 所以说到这儿你已经知道怎么解决了 ^_^* 

如果你不需要特定的语音包, 只需要

- 选中某一个语音

- 鼠标右键选择导出

- 修改导出的 regedit 注册表文件内容(将里面的语音包名字随意更改就好了)

- 运行修改后的 regedit 注册表文件

这样索引就不会越界啦 ^_^*!

---

## 另一种情况

不过, 当你查看过注册表以后, 发现索引没有越界(语音包的数量是正确的), 那么这个时候你面对的问题, 实际上是另外一个问题了, 你需要打开下面的链接看看.

[pyttsx3 支持的语音包](https://pyttsx3.readthedocs.io/en/latest/support.html)

---

# Refer

- [pyttsx3 official document](https://pyttsx3.readthedocs.io/en/latest/index.html)

- [pyttsx3 is not changing the voice? What to do now?](https://github.com/nateshmbhat/pyttsx3/issues/187)

- [Supported synthesizers](https://pyttsx3.readthedocs.io/en/latest/support.html)
