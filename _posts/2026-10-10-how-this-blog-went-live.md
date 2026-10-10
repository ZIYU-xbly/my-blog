---
layout: post
title: 项目复盘：这个博客是怎么上线的
date: 2026-10-10 08:30:00 +0800
excerpt: 从克隆模板到拿到一个能访问的网址，真正需要理解的只有几个概念。
tags:
  - 项目
  - 复盘
---

这个博客是基于一个 Jekyll 模板改的。整个过程比我想象中短，但有几个地方如果不理解，就会卡很久。

## 整条链路

```text
我在 _posts/ 里写 .md 文件
        ↓
git push 到 GitHub 的 main 分支
        ↓
GitHub Actions 运行 Jekyll，把 Markdown 编译成 HTML
        ↓
生成的静态文件被部署到 GitHub Pages
        ↓
得到一个可以访问的网址
```

## 卡住我的三个点

### 1. 项目站点的 baseurl

这个博客的地址不是根域名，而是 `https://ZIYU-xbly.github.io/my-blog/`，所有资源路径前面都要带上 `/my-blog`。

模板里的链接用了 `relative_url` 过滤器，它会把 `baseurl` 拼在前面。所以只要在 `_config.yml` 里写对就行：

```yaml
url: "https://ZIYU-xbly.github.io"
baseurl: "/my-blog"
```

如果少了这一行，首页还能打开，但样式表会 404，页面会退化成没有样式的纯文本。

### 2. 文章日期不能是未来

Jekyll 默认**不构建未来日期的文章**。日期写成明天，文章就会安静地消失，构建日志里也不一定有明显的报错。

### 3. 仓库的 Pages 来源要选对

必须在 `Settings → Pages → Source` 里选 **GitHub Actions**，而不是"从分支部署"。选错了，推代码之后不会触发这套 workflow。

## 我做对的地方

- 配色全部集中在 `:root` 变量里，改主题只动一段
- 没有引入任何框架和外部 CDN，页面加载不依赖第三方
- 每篇文章都写了 `excerpt`，首页卡片才能显示摘要

## 下一步

想给博客加一个文章归档页，把所有文章按时间排在一页上。不过在那之前，先多写几篇再说。
