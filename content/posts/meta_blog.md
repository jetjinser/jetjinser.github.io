+++
title = "使用 zola 和 github pages 搭建静态博客"
date = 2021-12-04

[taxonomy]
categories = ["article"]
tags = ["blog"]
+++

> 2022-06-22 更新：过时的文章，而且没写完，有空再更新。

## 0x00 介绍
zola 是一个静态网站生成器，我们用它来生成网页，托管在 github pages 上。

> github: https://github.com/getzola/zola
>
> Home page: https://www.getzola.org

托管在 github pages 上的好处是，可以很方便地用 github actions 来构建和部署博客。
也就是说，生成博客的部分，都在云上完成了，我们在本地写文章，只需要关心内容就好了。

虽然理论上本地不需要安装 zola，但为了方便调试和配置，还是需要先 [安装](https://www.getzola.org/documentation/getting-started/installation/)，在本地配置。

## 0x01 新建仓库
从 [github pages](https://pages.github.com) 继续：
按照文档中的步骤，先创建一个名为 <username>.github.io 的仓库，如我的是 https://github.com/jetjinser/jetjinser.github.io。

新建出来的是一个空的仓库，把它 clone 到本地：
```bash
git clone https://github.com/<username>/<username>.github.io
```
接下来的所有操作都在这个文件夹内进行：
```bash
cd <username>.github.io
```

## 0x02 配置本地环境

### init
初始化 zola 只需要在一个空文件夹内执行下面这句指令：
```bash
zola init
```

按照交互式的说明，选择 yes 或 no 就行，这些选项后续都可以在 `config.toml` 里手动修改，所以随便回答也可以。

执行完毕后除了 `config.toml`，还会生成几个文件夹，如果没有自己定制主题的需求，只要关注这几个文件夹：
- `content/`: 存放所有的文章
- `static/`: 存放静态资源
- `themes/`: 存放主题

### theme
如果没有配置主题和模版，zola 就不知道该如何生成网站。
主题其实就是已经配置好了的模版，嫌麻烦直接用就好了。
zola 文档列出了 [所有主题](https://www.getzola.org/themes/)

每个主题要求的配置不太一样，具体需要查看文档说明。

这里随便找了一个 [anpu](https://www.getzola.org/themes/anpu/) 主题。
推荐用 git submodule，不要按照文档里的直接 clone。
```bash
git submodule add https://github.com/zbrox/anpu-zola-theme.git themes/anpu
```
其余的按照文档配置即可。

### content
接下来可以填写正文了。
首先要在 `content/` 文件夹下创建一个 `_index.md` 文件。这是用来组织 `pages` 的 `section`。
```markdown
+++
paginate_by = 5
sort_by = "date"
+++
```
这里的 `+++` 包裹起来的内容，被 zola 称为 [Front matter](https://www.getzola.org/documentation/content/section/#front-matter)，里面是 TOML 语法，
如果用 `---` 包裹起来，里面就是 YAML 语法。zola 推荐使用 TOML。
相当于每篇文章写配置的地方。

先写一篇 `content/test.md` 试一下：

```markdown
+++
title = "Bonjour"
date = 2021-12-03

[taxonomies]
categories = ["article"]
tags = ["shots"]
+++

这里正常填写正文...
```

我们可以用 `zola build` 在 `public/` 文件夹下生成网站，也可以直接用 `zola serve` 开一个本地的监听。
我们跑 `zola serve` 试一下，如果没有报错，在浏览器打开 `http://127.0.0.1:1111` 就可以看到你的网站了。

本地的博客配置基本完成了。

## 0x03 配置 GitHub actions
https://www.getzola.org/documentation/deployment/github-pages/

## 0x04 自定义域名
`CNAME` 放在 `static/` 内。

## 0x05 总结
原始内容在 master 分支，GitHub actions 自动生成静态网页到 gh-pages 分支。
