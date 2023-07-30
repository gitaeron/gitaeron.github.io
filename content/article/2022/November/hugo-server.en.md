---
title: "Previews problem while running hugo server"
date: 2022-11-27T16:44:00+08:00
author: "Aeron"

readingTime: 5
categories: ["tech"]
tags: ["Hugo", "Bilberry Theme"]
---


# What happened

There's some operation caused 404 page while running `hugo server` .

Once you changed `categories` and `tags` during running `hugo server` , browser'll jumped to 404 page after you click the changed `categories` and `tags`.

# Multiple pictures contained

<!--more-->

<br/>

---

<br/>

# Process description

For example:

1. This article's `categories` and `tags` is like this

- categories: ['tech']
- tags: ['hugo', 'issue', 'experience']

![1](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/1.png)

<br/>

2. After running `hugo server` , you can simply click `categories` or `tags` jump to corresponding page

![2](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/2.png)

<br/>

3. But after you changed `categories` or `tags` in editor
   
![3](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/3.png)

<br/>

![4](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/4.png)

<br/>

4. Turns out with 404

![5](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/5.png)

<br/>

5. When you're running `hugo server` , this situation includes but is not limited to `categories` or `tags` or `author` etc 

![6](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/6.png)

<br/>
   
6. It also works for adding new one after running `hugo server`

![7](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/7.png)


<br/>

---

<br/>

# Solution

**Just simply `Ctrl` + `C`**

**Then running `hugo server`**

![8](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/hugo-server/8.png)

<br/>

---

<br/>

# Conclusion

I don't know why, but it is as it is....

I‘m not even a front-end engineer anyway.... &emsp; :(

Is this article like a kind of simply tips?

