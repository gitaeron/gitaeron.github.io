---
title: "Installing Bilberry Theme"
date: 2023-05-18T15:32:00+08:00

categories: ["技术", "教程"]
tags: ["Hugo", "Bilberry Theme", "translate by chatgpt-3.5"]
author: "Lednerb"
---

您将在 [官方Github存储库](https://github.com/Lednerb/bilberry-hugo-theme) 中找到所有有关如何在您的Hugo网站中设置此主题的信息.

<!--more-->

**如果您想要安装此主题,请按照以下步骤进行操作:**

- 安装 Hugo 并创建新网站

```plaintext
hugo new site my-new-blog
```

- 切换到您的主题文件夹并导入Bilberry主题的最新版本

```plaintext
cd my-new-blog/themes
git clone https://github.com/Lednerb/bilberry-hugo-theme.git
```

- 将示例内容复制到您的新站点

```plaintext
cp -r bilberry-hugo-theme/exampleSite/* ../
```

- 测试安装

```plaintext
cd ../
hugo server -D
```

- 根据您的需要配置config.toml文件

- 开始博客撰写
