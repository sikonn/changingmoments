---
title: HUGO & MemE | 踩坑实录
date: 2022-05-02T01:35:26.079Z
draft: true
lastmod: 2022-05-02T02:26:49.229Z
description: ""
tags:
  - HUGO
  - MemE
---
这个博客建立了6天，我已经踩了无数坑。鉴于MemE主题的帖子比较少，我也写一些自己的配置！

写博客的原因很简单，我需要一个**能写很多字/确保不丢失/没有审查/被看到**的平台，多亏了[Missing不想睡](https://hugo-missingid.vercel.app/)的帮助和[塔塔](https://mantyke.icu/)的教程，我才接触到了HUGO静态博客。

本贴又我和[Missing不想睡](https://hugo-missingid.vercel.app/)的聊天记录整理而成！

### HUGO

#### 安装HUGO

务必确认好hugo.exe的路径添加到环境变量

#### 配置SSH

***生成SSH***
虽然大家应该没有我这么笨，但是$符号复制塔塔的代码的时候记得删掉，我当时没有删掉就not found了
***连接SSH***
如果没有直接显示successful，yes一定要好好输入，我当时一时手快又显示不了了

#### 安装主题

[塔塔](https://mantyke.icu/)的三步一定要好好跟着做，下载主题，解压，重命名为meme，复制到themes里再把config文件复制回根目录。

**要是错的实在受不了了，爆破重开会更好一点，我就是第二遍顺畅的完成了。**

#### 写文章

如果想简化一点，可以跟着[Missing不想睡](https://hugo-missingid.vercel.app/)的这一篇[forestry.io 进行博客写作](https://hugo-missingid.vercel.app/p/forestry/)进行。

用forestry.io需要注意的是一定要把GitHub转成公开！

我目前主要用的是vscord中的front matter插件，主要是看了[HUGO gastby nextjs zola 等静态博客可视化编辑器 front matter | forestry.io | 像WORDPRESS一样轻松编写 markdown](https://www.youtube.com/watch?v=s1Gdu4RZDp4&t=303s)的视频，觉得也很好用，现在两边都有用。

### MemE

**最需要注意的是，如果你发vercel不给你发邮件了/修改的内容显示不上了，去检查vercel是不是出现了error，如果是的话你点进去看看到底是哪里error了改了就可以了。**

#### 添加图片

就是说我也没想到我添加图片都不会，添加图片的话frontmatter会帮你生成markdown链接，也可以再forestry中直接添加。

如果你想修改正文图片大小，可以用下面这个代码
```
<p align="center">
    <img src="图片链接" width="480" />
</p>
```
图片地址可以是html也可以是相对路径，后面的大小可以随意改动。

如果你出现了添加图片显示不了的情况，去config文件中找到**baseurl**，将他改成你自己的网址，还有后面的**图片外链地址**和**视频外链地址**也要一并改掉（找不到的话直接搜索就可以了）

#### 全站字数统计

MemE自带文章字数统计，但是没有全站字数统计，但是真的很想知道自己一共写了多少字，所以加上了这一部分。

在footer.html中改动，需要将config中的页脚关闭，否则会重叠。

```
<center>
    <section class="copyright">
        &copy; 
        {{ if and (.Site.Params.footer.since) (ne .Site.Params.footer.since (int (now.Format "2022"))) }}
            {{ .Site.Params.footer.since }} - 
        {{ end }}
        {{ now.Format "2022" }} {{ .Site.Title }}
        <br/>
        共 {{ len (where .Site.RegularPages "Section" "posts") }} 篇文章
        {{$scratch := newScratch}}
        {{ range (where .Site.Pages "Kind" "page" )}}
            {{$scratch.Add "total" .WordCount}}
        {{ end }}
        共嘟嘟了{{ div ($scratch.Get "total") 1000.0 | lang.FormatNumber 2 }}k字.
    </section>
</center>
```

#### 菜单栏

因为觉得标签和分类功能重叠，我整个删掉了分类版块。因为不知道友链怎么设置，在content里加入了friends板块，用超链接实现友链功能。（可能之后还会去研究一下怎么搞真正的友链吧）
```
---
title: Friends
date: 2022-04-28T19:06:26+08:00
draft: true
menu: main
weight: 60
comment: false
lastmod: 2022-05-01T15:05:39.756Z
description: 
---
```
只要添加menu：true就可以在菜单栏显示了！如果以后还想添加什么也可以照这个方法做。

#### 烟花特效

一些没用的小功能，但是暗黑模式的时候很好看！
放置在/layouts/partials/custom/script.html中。
```
<!--鼠标点击特效，烟花效应-->
<script src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/static/mouse-click.js"></script>
<canvas width="1777" height="841" style="position: fixed; left: 0px; top: 0px; z-index: 2147483647; pointer-events: none;"></canvas>
```

#### 图标

这里的图标是指打开的时候在网站名字左边显示的东西，比如说我的是我画的月亮。
这个放置在themes\meme\static中，命名为favicon.ico就可以了！

