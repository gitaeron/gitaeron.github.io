---
title: "An issue caused by using `series` with Multi Language and cause unexpected behavior"
date: 2022-11-30T04:58:00+08:00
author: "Aeron"

readingtime: 6
categories: ["Technology"]
tags: ["Bilberry Theme"]
---

Now It's fixed at `bilberry-hugo-theme` @v3.15.0

---

{{<toc>}}

<!--more-->

---

## First of all

Before I create a new issue or make a pull request with my code, I just want  to discuss this issue first to make sure that I wasn't misunderstanding origin code.

Cause I'm not a front-end or back-end developer, and I'm a new in IT : (

So I started a discuss in this neighborhood : )



---

## Environment

- Windows 10, - latest hugo

- github page

- Theme Version
  - Just latest `Bilberry-hugo-theme` at @5d7f51ba2e174ea45404595fef24f5996e395672

- Multi Language
  - I choose English and Chinese, English as default.

![1](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/1.png)

I changed `zh-CN.toml` to `cn.toml` .

And the content below is the process of how I fix that.

I'll make a pull request after it could be a issue and it's a properly way to fix it.

---

## What happened ?

Issue founded in languages which are not default.

If there's Multi Language, the URL of `series` is like this:

- /series

- /Second-Language/series

- /Third-Language/series

- /...../series

But the fact is like this, it will jump to 404 page because URL Error:

</br>

![2](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/2.png)

</br>

So I searched the project and located in `/bilberry-hugo-theme/layouts/_default/series.terms.html`

</br>

![3](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/3.png)

</br>

That's the reason why the URL is wrong!

> As you see, I suppose it's also effected on any languages which are not the default language.
> In this scene, is cn.

So if we can just type the correct URL by self.......

That's impossible lol : )

</br>

![4](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/4.png)

</br>

In fact, I have no idea at first. It's a totally unknown area for me. I almost give up after couple days.

But after I studied similar html inside bilberry theme, I found a thing named `relLangURL` .

`relLangURL` is used to add /language-name/  before your URL.

For example, in my case:

- {{ $origin }}  => is point to /series/a.md

after you use `relLangURL`:

- {{ $origin | relLangURL }} => is point to /cn/series/a.mds

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

**👈👆 Tips: How about we expand the width.... 👆👉**

I assumed that this code is for single language user.

`{{ $series_path := (printf "/series/%s" (urlize $name)) }}`

However that's the most case. Maybe the author is coded by purpose, or based on some reason. I don't know, I'm just a noob.

> For my perspective, I think it's more inclusive to do so: {{ $series_path | relLangURL }}

Anyway this is a experience, I wish it could helped no matter this can or can't be a `issue` or something, at lease people will searched it in general if they need it, my title is so long lol : )

---

## Unexpected behavior

But this is not over. 

This `Unexpected behavior` is not concerned about adding `relLangURL` in `series.terms.html` .

The problem is about `series shortcode`(series list) didn't display / show / appear.

</br>

![5](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/5.png)

</br>

![6](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/6.png)

</br>

---

First, I located in `/bilberry-hugo-theme/layouts/shortcodes/series.html` .

</br>

![7](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/7.png)

</br>

![8](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/8.png)

</br>

The problem caused by specify language itself.

It puts me in the same situation like above case. Took some days.

Then I found `urlize` to solved this.

</br>

![9](https://raw.githubusercontent.com/gitaeron/gitimg/main/gitaeron.github.io/technology/multi-lan-series-issue/9.png)

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

**👈👆 Tips: How about we expand the width.... 👆👉**

In fact the `$series_name` is already used `urlize` at the first line, that's the reason why they didn't equals.

I also doesn't know why.

---

## Conclusion

Because I don't know why this, 

so I fixed maybe +_+  or it's just fixed in a possibly inappropriate way.

