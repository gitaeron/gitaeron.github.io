---
title: "多语言下使用 `series` 出现 issue 及多语言造成的未预期行为 - Fixed"
date: 2022-12-06T17:08:00+08:00
author: "澜冬雪"

readingtime: 10
categories: ["技术"]
tags: ["issue", "Bilberry Theme"]

series: ['技术系列']
---

已在该版本 `bilberry-hugo-theme` @v3.15.0 中被修复.

---

{{<toc>}}

<!--more-->

---

## 首先

在我创建一个新的 `issue` 或发起一个 `pull request` 之前, 我希望先创建一个关于本次内容的 `discussion` 以确保我没有误解原代码.

因为我不是前端或后端开发人员, 我只是个小白.

所以我在社区中开启了这个讨论.

---

## 环境

- Windows 10, - latest hugo

- github page

- 主题版本
  - Just latest `Bilberry-hugo-theme` at @5d7f51ba2e174ea45404595fef24f5996e395672

- 多语言
  - I choose English and Chinese, English as default.

![1](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/1.png)

我将中文语言文件 `zh-CN.toml` 改变为 `cn.toml` .

接下来的内容将展示我如何修复本次主题提及的问题及未预期行为.

当确定在文章中展示的修复方法是一个可行的,适当的方法后我会发起一个 `pull request`.

(其实在 ~~翻译~~ 写中文文章的时候, 已经被修复了.)

---

## 发生了什么 ?

问题发生在多语言情况下的非默认语言.

如果你使用了多语言, 那么关于 `series` 的 URL 应该是这样的:

- /series

- /第二语言/series

- /第三语言/series

- /.../series

但实际上的 URL 却变成了这样, 并且因为 URL 的错误它会跳转到 404页面

</br>

![2](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/2.png)

</br>

所以我搜索了整个工程项目,并定位到 `/bilberry-hugo-theme/layouts/_default/series.terms.html`

</br>

![3](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/3.png)

</br>

这是造成 URL 错误的原因!

> 如你所见, 对于造成该错误的原因, 我推测这个错误应该适用于其他的非默认语言.
> 对于这儿的例子,是 cn

~~所以如果我们手动输入正确的 URL 就可以~~.....

这是不可能的 : )

</br>

![4](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/4.png)

</br>

起初我是不知道应该怎么办的, 因为这对我来说是一个全新的领域, 在折腾了几天过后我几乎要放弃了.

但是后来, 我在主题工程文件中研究了相似的 html 文件, 然后我发现了这个 => `relLangURL`.

`relLangURL` 被用来添加 /语言名/ 在你的URL 之前.

比如, 对于本次的例子来说:

- {{ $origin }} => 指向 /series/a.md

使用 `relLangURL` 之后, URL 的链接变为:

- {{ $origin | relLangURL }} => 指向 /cn/series/a.md

---

**About this code here**

``` html
{{ define "main" }}

<div class="content">
    <div class="article-wrapper u-cf single">
        <a class="bubble" href="{{ "/series/" | relLangURL}}">
            <i class="fa fa-fw fa-list-alt"></i>
        </a>

        <article class="article">
            <div class="content">
                <h3>{{ i18n "series" | default "series" }}</h3>
                <hr>
                <ul id="all-series">
                    
                    {{ range $name, $taxonomy := .Site.Taxonomies.series }}
                        {{ $series_name := $name }}
                        {{ $series_path := (printf "/series/%s" (urlize $name)) }}
                        {{ $series_page := site.GetPage $series_path }}

                        {{ if $series_page }}
                            {{ $series_name = $series_page.Title }}
                        {{ end }}
                        
                        <!--add relLangURL here-->
                        <li><a href="{{ $series_path | relLangURL }}">{{ $series_name }} ({{ $taxonomy.Count }})</a></li>
                    {{ end }}
                </ul>
            </div>
        </article>
    </div>
</div>
{{ end }}
```

**👈👆 Tips: 我们把文章宽度拓宽怎样? 👆👉**

我假设这儿的原代码是在单语言的情况下写的.

`{{ $series_path := (printf "/series/%s" (urlize $name)) }}`

因为下文没有添加 `relLangURL`, 所以相当于这儿被写死了, 对于 `series` 的URL被固定为 /series/%s 而没有前面的语言名指向特定语言的分目录下的系列.

不过确实, 单语言的使用者应该更多. 或者作者在这儿这么写别有深意, 我也不知道....

> 我的意见是: 我认为在这儿添加 `relLangURL` 更具有包容性

不管怎样, 这算是一个经验. 我希望能帮助到其他人. 至少我的标题起的足够长,让人能根据适当的关键字检索到这篇文章.

---

## 未预期行为

但是这还没有结束.

这个 `Unexpected behavior` 与上面提及的在 `series.terms.html` 添加的 `relLangURL` 并不相关.

这个问题是关于使用 `series shortchde` (series list) 没有显示内容.

具体看下图:

</br>

![5](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/5.png)

</br>

![6](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/6.png)

</br>

---

首先, 问题定位到 `/bilberry-hugo-theme/layouts/shortcodes/series.html` .

</br>

![7](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/7.png)

</br>

![8](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/8.png)

</br>

这个问题由特定的语言引起. (根据大佬的调查, 是由包含 non-ASCII 字符引起的, 实际上也确实是这个现象, 不过跟原代码中具体的实现有关)

我找到了 `urlize` 来解决这个问题.

</br>

![9](https://raw.githubusercontent.com/gitaeron/gitimg/main/article/multi-lan-series-issue/9.png)

</br>

**About this code here**

``` html
{{ $series_name := .Get 0 | urlize }}

{{ range $key, $taxonomy := .Site.Taxonomies.series }}

    <!--add urlize here-->
    {{ if eq ($key | urlize) $series_name }}
    <ul>
        {{ range $taxonomy.Pages.ByDate }}
        <li hugo-nav="{{ .RelPermalink }}"><a href="{{ .Permalink }}">{{ .LinkTitle }}</a></li>
        {{ end }}
    </ul>
    {{ end }}
{{ end }}
```

**👈👆 Tips: 我们把文章宽度拓宽怎样? 👆👉**

事实上, 在最开始的代码, $series_name 已经使用了 `urlize` 进行转换, 但是在下面的 `if eq $key $series_name` 中进行比较的时候, 并未使用 `urlize` 将 $key 进行转换, 所以会造成上面两张图的错误.

我挺好奇为什么不在下面的 $key 也加上 `urlize`, 或者将第一行代码中的 `urlize` 删除, 在下面的 `if eq $key $series_name ` 中同时使用或同时不使用.

---

## 结论

因为我不是前后端开发人员,所以我不知道我做的对不对...

