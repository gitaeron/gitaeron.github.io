---
title: "Git 多用户配置"
date: 2023-08-13T17:30:00+08:00

author: "澜长生"
categories: ["技术"]
tags: ["Git"]

readingTime: 5
---

{{<toc>}}

<!--more-->

---

## 密钥

- 生成密钥

```bash
ssh-keygen -t rsa -C "EMail"s
```

```bash
(base) [15:55:07] [~] ❱❱❱ ssh-keygen -t rsa -C "youremail@gmail.com"

Generating public/private rsa key pair.

Enter file in which to save the key (/home/snoer/.ssh/id_rsa):
```

---

- 在 github 设置中添加密钥

```bash
cat  name_rsa.pub
```

---

## config文件

- 找到 `.ssh`文件夹, 添加config文件

```bash
cd /home/user_name/.ssh/
vim config
```

- 文件内容

```bash
Host aeron
  HostName github.com
  User aeron
  IdentityFile ~/.ssh/aeron_rsa

Host snoer
  HostName github.com
  User snoer
  IdentityFile ~/.ssh/snoer_rsa
```

---

- 参数说明

1. `Host`

主机别名, 可以自定义.

2. `HostName`

服务器真实地址

3. `User`

用户名, 随意.

4. `IdentityFile`

私钥文件路径

5. `PreferredAuthentications`

认证方式, 可不填写

---

- 配置检查

```C
ssh -T git@aeron
```

