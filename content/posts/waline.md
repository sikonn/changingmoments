---
title: HUGO&MemE&Waline | 踩坑实录
date: 2022-05-05T07:13:16.516Z
draft: true
lastmod: 2022-06-27T16:13:22.656Z
layout: post
tags:
  - HUGO
  - MemE
  - Waline
---

### 写在前面

**注意不要在meme文件夹中直接修改，要改什么都在根目录下添加。**

### 参考资料

[Waline官方文档-快速上手](https://waline.js.org/guide/get-started.html)

[Waline 评论系统的介绍与基础配置](https://guanqr.com/tech/website/introduction-and-basic-setting-of-waline/)

[Hugo | 为 Blog 增加评论区](https://mantyke.icu/2021/comment/)

[博客装修：删删改改大胆行事](https://gregueria.icu/posts/decoration/)

还有人美心善的[Missing不想睡](https://hugo-missingid.vercel.app/)大半夜还在帮我看源代码！

### 具体步骤

#### 插入评论

首先看[Waline官方文档-快速上手](https://waline.js.org/guide/get-started.html)，leancloud设置和vercel很简单，设置域名可以略过，这里需要注意的是html导入的位置，各个主题的位置可能不一样，MemE主题放在了layouts\layouts\third-party\waline.html，**记得改动serverurl**，完成之后记得注册。

#### 插入主题

接下来看[Waline 评论系统的介绍与基础配置](https://guanqr.com/tech/website/introduction-and-basic-setting-of-waline/)，主要看插入meme主题的部分，需要注意的是
> ~/layouts/partials/components/comments.html
~/layouts/layouts/third-party/script.html
~/layouts/layouts/third-party/waline.html

这三个文件夹需要在根目录新建！千万不要跑到meme里面改动！

因为这个文档内容并不是最新的，他的github里有最新内容，我贴在这里

**comments.html**
```
{{ if and (.Params.comments | default .Site.Params.enableComments) (eq hugo.Environment "production") }}
    {{ if or (in .Site.Params.mainSections .Section) .Params.comments }}

        {{ if not .Site.Params.autoLoadComments }}
            <div class="load-comments">
                <div id="load-comments">{{ i18n "loadComments" }}</div>
            </div>
        {{  end }}

        {{ if .Site.Params.enableDisqus }}
            <div id="disqus_thread"></div>
        {{ end }}

        {{ if .Site.Params.enableValine }}
            <div id="vcomments"></div>
        {{ end }}

        {{ if .Site.Params.enableWaline }}
            <div id="waline"></div>
        {{ end }}

        {{ if .Site.Params.enableUtterances }}
            <div id="utterances"></div>
        {{ end }}

        {{ if .Site.Params.enableGitalk }}
            <div id="gitalk-container"></div>
        {{ end }}

    {{ end }}
{{ end }}
```

**script.html**
```
{{ if .Params.katex | default .Site.Params.enableKaTeX }}
    {{ partial "third-party/katex.html" . }}
{{ end }}

{{ if .Params.mathjax | default .Site.Params.enableMathJax }}
    {{ partial "third-party/mathjax.html" . }}
{{ end }}

{{ if .Params.mermaid | default .Site.Params.enableMermaid }}
    {{ partial "third-party/mermaid.html" . }}
{{ end }}

{{ if and (.Params.comments | default .Site.Params.enableComments) (eq hugo.Environment "production") }}
    {{ if or (in .Site.Params.mainSections .Section) .Params.comments }}

        {{ if .Site.Params.enableDisqus }}
            {{ partial "third-party/disqus.html" . }}
        {{ end }}

        {{ if .Site.Params.enableValine }}
            {{ partial "third-party/valine.html" . }}
        {{ end }}

        {{ if .Site.Params.enableWaline }}
            {{ partial "third-party/waline.html" . }}
        {{ end }}

        {{ if .Site.Params.enableUtterances }}
            {{ partial "third-party/utterances.html" . }}
        {{ end }}

        {{ if .Site.Params.enableGitalk }}
            {{ partial "third-party/gitalk.html" . }}
        {{ end }}

    {{ end }}
{{ end }}

{{ if .Site.Params.enableMediumZoom }}
    {{ partial "third-party/medium-zoom.html" . }}
{{ end }}

{{ if .Site.Params.enableInstantPage }}
    {{ partial "third-party/instant-page.html" . }}
{{ end }}

{{ partial "third-party/busuanzi.html" . }}

{{ partial "custom/script.html" . }}
```

**waline.html**
```
 <script>
    function loadComments() {
        if (typeof Waline === 'undefined') {
            var getScript = (options) => {
                var script = document.createElement('script');
                script.defer = true;
                script.crossOrigin = 'anonymous';
                Object.keys(options).forEach((key) => {
                    script[key] = options[key];
                });
                document.body.appendChild(script);
            };
            getScript({
                src: 'https://cdn.jsdelivr.net/npm/@waline/client/dist/Waline.min.js',
                onload: () => {
                    newWaline();
                }
            });
        } else {
            newWaline();
        }
    }
    function newWaline() {
        const locale = {
            nick: "昵称*",
            mail: "邮箱*",
            placeholder: "请填写正确的昵称和邮箱，方便接收评论回复信息。若随意填写邮箱地址，发布包括但不限于广告、攻击性、无意义等内容文字，将会视该评论为垃圾评论，作删除处理。"
        };
        new Waline({
            el: '#waline',
            dark: 'html[data-theme="dark"]',
            serverURL: '{{ .Site.Params.walineServerURL }}',
            wordLimit: {{ .Site.Params.walineWordLimit }},          
            meta: {{ .Site.Params.walineMeta }},
            pageSize: {{ .Site.Params.walinePageSize }},
            lang: '{{ .Site.Params.walineLang }}',
            highlight: {{ .Site.Params.walineHighlight }},
            emoji: {{ .Site.Params.walineEmoji }},
            requiredMeta: {{ .Site.Params.walineRequiredMeta }},
            login: '{{ .Site.Params.walineLogin }}',
            copyright: {{ .Site.Params.walineCopyright }},
            locale
        });
    }
</script>
```
还有要在config.toml中**评论的位置**加入

```
    ## Waline
    enableWaline = true
    walineServerURL = "https://comments-sikonn.vercel.app/"
    walineWordLimit = 0
    walineMeta = ["nick", "mail", "link"]
    walinePageSize = 10
    walineLang = "zh-CN"
    walineHighlight = true
    walineEmoji = ["https://cdn.jsdelivr.net/gh/walinejs/emojis@1.0.0/weibo", "https://cdn.jsdelivr.net/gh/walinejs/emojis@1.0.0/bilibili"]
    walineRequiredMeta = ["nick", "mail"]
    walineLogin = "enabled"
    walineCopyright = true
    # 说明：https://waline.js.org/
```

#### 改变样式

改变样式这里参考了[Hugo | 为 Blog 增加评论区](https://mantyke.icu/2021/comment/)，注意meme主题应放在layouts\partials\footer.html下

#### 邮箱提醒

这里请看[博客装修：删删改改大胆行事](https://gregueria.icu/posts/decoration/)的教程，写的非常详细，我都不会出错！

### 写在最后

增加评论对我来说是心血来潮，没想到快给我整麻了，昨天一天都坐在电脑前没有进展（犯了大大小小的错误），整个人麻木的刚刚改过就不知道自己改的是什么东西。还把自己的暗黑模式整没了，今天重开了一遍就成功了！

**对我来说的感想可能是**

错过的一点点小细节都会产生影响，不要想当然，仔细看，认真做！

要有清晰的目标，知道自己在干嘛。现在的博客出了什么问题，要改成什么样子，列出list就会很快解决。

不要放弃，也要敢于重新开始！

