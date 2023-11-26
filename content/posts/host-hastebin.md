+++
title = "self-hosted hastebin"
date = 2023-11-26

[taxonomy]
categories = ["article"]
tags = ["blog", "self-hosted"]
+++

# 也许是便宜 vps 最实用的 self-hosted 服务

前几天阿里云发短信给我推销了99元/年的 1c2g vps，不能让它闲着。

## Pastebin

self-hosted 是个热门话题，我也折腾过不少服务，但是最后总会感到是在自己创造需求；很少有能在平时真的会很自然地用到的。
直到我意识到我每天都在使用的一个服务 -- **pastebin**：

> A pastebin or text storage site is a type of online content-hosting service where users can store plain text.

我常用 pastebin 的地方：
- 只是需要某个区域输入文本。
- 临时记一下某段 snippet 或数据。
- 贴上长段 log 方便 debug。
- 分享大段代码到群组。

有不少在线 pastebin 服务，比如 #1 的 [pastebin.com](https://pastebin.com/)，
一些社区比如 [debian](https://paste.debian.net/)，[mozilla](https://pastebin.mozilla.org/) 也会维护。

但他们要么是页面太丑，操作复杂，要么是有恼人的登陆，特别是 [ubuntu](https://pastebin.ubuntu.com/) 的 pastebin 甚至要求登陆才能上传文本。
最好用的 pastebin 应该是 [fars.ee](https://fars.ee/) 和 [sprunge](http://sprunge.us/)
以及我想 self-hosted 的 hastebin。

## Hastebin
我并不了解 Hastebin 的历史，只是在一次偶然发现了 [hastebin.skyra.pw](https://hastebin.skyra.pw/)。

这里总结一下我为什么偏好它：
- 打开就能直接编辑（类似 [paste.sh](https://paste.sh/), 这里得提一句 paste.sh 也很好用，但是它没有高亮，且有我不太喜欢的自动更新）
- 页面很好看，没有多余的选框
- 没有烦人的登录和账号
- 没有几乎派不上用场的 history，
- 以及适当的高亮。

不过我后来发现，hastebin 官方 host 的域名应该是 [hastebin.com](https://hastebin.com)，
而它已经 301 到 [toptal.com](https://www.toptal.com/developers/hastebin) 了。

从[仓库](https://github.com/toptal/haste-server)的 [issue#492](https://github.com/toptal/haste-server/issues/429) 来看，确实已经被 toptal 收购了，接着它似乎就走向了开源社区的死亡。

`hastebin.skyra.pw` 和专门的 pastebin 域名不太一样，就像前面提到的 debian 和 mozilla 一样，它确实是由一个社区 host 的。
从 [about](https://hastebin.skyra.pw/about) 中可以看到：
> The HasteServer image used to provide a reliable, self-controlled and self-hosted HasteServer for Skyra

打开 [skyra.pw](https://skyra.pw/) 也可以看到 skyra 是一个 discord mod bot。

skyra 项目需要一个稳定好用的 pastebin，而 haste-server 已经过时，无人维护，于是他们重新维护了一个更新的仓库，
用来提供他们的需要。(谢谢 skyra)

## How to serve

最简单的方式就是使用 docker compose 启动，并且这应该也是推荐的做法，应该会更安全一点？

### 前置要求
- 机器上装有 docker compose。

### 下载和启动

随便创建一个目录：
```fish
mkdir haste-server
cd haste-server
```

从 skyra 维护的 haste-server 仓库中下载 docker-compose.yml：
```fish
curl -LO https://raw.githubusercontent.com/skyra-project/haste-server/main/docker-compose.yml
```
很短，在这里贴出来：
```yaml
version: '3.9'
services:
  redis:
    container_name: redis
    image: redis:alpine
    restart: always
    ports:
      - '8287:8287' # <-- redis 的端口
    command: redis-server --port 8287 --requirepass redis

  haste:
    container_name: hasteserver
    image: ghcr.io/skyra-project/haste-server:latest
    build: .
    restart: 'no'
    depends_on:
      - redis
    ports:
      - '8290:8290' # <-- haste-server 从 docker 中暴露出的端口
    environment:
      PORT: 8290 # <-- haste-server listen 的端口
      STORAGE_TYPE: redis
      STORAGE_HOST: redis
      STORAGE_PORT: 8287
      STORAGE_PASSWORD: redis
      STORAGE_DB: 2
      STORAGE_EXPIRE_SECONDS: 21600
```

要注意的地方只有 redis 和 haste-server 的端口，
前者如果不想暴露出来可以直接删除那两行，后者随意。

没有多余的设置，直接通过 docker compose 启动：
```fish
docker compose up -d
```
等 docker pull 下 image，就可以通过 `localhost:<haste-server port>` 访问了，如果没有修改过端口，那么默认就是 `localhost:8290`。

如果 redis 端口有暴露出来，可以通过 `redis-cli` 访问来查看存储的数据：
```
redis-cli -p 8287 -a redis -n 2
```

`-p` 指定端口，`-a` 指定密码，`-n` 指定数据库编号。

一些简单的操作：
```
keys
```
列出所有 `key`。

```
get <key>
```
获取 `key` 的 `value`。

```
append <key> "any string"
```
在 `key` 的 `value` 后加上 `any string`。

> INFO: 如果你想修改 `localhost/about` 的文字，可以通过修改 `about` `key` 的 `value` 来实现。

### 开放服务
我使用 cloudflare 的 tunnel 服务暴露出 haste-server，

这不是本文的重点，只作简单记述一下：
```fish
cloudflared tunnel create chez-ali
cloudflared tunnel dns chez-ali *.purejs.icu
```

`cloudflared/config.yaml`：
```yaml
tunnel: 4732xxxx-xxxx-xxxx-xxxx-xxxxxxxxe044
credentials-file: /home/jinser/.cloudflared/4732xxxx-xxxx-xxxx-xxxx-xxxxxxxxe044.json

ingress:
  - hostname: note.purejs.icu
    service: http://127.0.0.1:2718
  - hostname: box.purejs.icu
    service: http://127.0.0.1:8000
  - hostname: hastebin.purejs.icu
    service: http://127.0.0.1:8290
  - service: http_status:404
```

## How to use

### 在浏览器中使用
只需要从浏览器直接打开 haste-server 的地址，比如我用的是 `hastebin.purejs.icu`，一切 ok。

### 通过终端使用

#### haste-client (不推荐)
hastebin 提供了一个名为 [haste-client](https://github.com/toptal/haste-client) 的命令行工具，由 ruby 编写。
不确认一般的包管理工具的默认仓库有没有，但是我试过 `brew`，可以直接通过它安装：
```fish
brew install haste-client
```
如何使用可以参考仓库的 README。
需要注意的是，haste-client 会构造出一个 `<domain>/share/xxx` 的 URL，
这个 `/share/` 路径在 skyra hastebin 中没有意义，正确的的 URL 应该是 `<domain>/xxx`。

#### curl
haste-client 的功能非常简单，事实上通过一个简单的 shell 脚本就可以实现相同的功能。

这里贴出 fish 的代码，其他的 shell 大同小异。
```fish
function hst
    set -q HASTE_SERVER
    and set -f server "$HASTE_SERVER"
    or set -f server 'https://hastebin.purejs.icu'

    if isatty stdin
        set -f json (curl -s "$server/documents" --data-binary @$argv)
    else
        cat $argv 2>/dev/null | read -l -z content
        set -f json (curl -s "$server/documents" --data-binary $content)
    end

    echo $json | sed "s|{\"key\"\:\"\(.*\)\"}|$server/\1\n|"
end

```
`hst` function 做了两件事情：
1. curl POST "$server/documents" (e.g. `https://hastebin.purejs.icu/documents`)，这里有两点可以说；
    1. curl 带 `--data-binary` 参数而不是 `--data` 参数，因为后者会删去所有的 newline。
    2. 当 `--data[-binary, -ascii]` 后跟随的参数以为 `@` 开头，curl 会将其之后的字符视为 filename，并从中读取内容。
2. 将 curl 返回的 json `{"key":"xxx"}` 用 sed 拼成 `$server/xxx`。

```fish
hst filename.txt
dmesg | hst
```
一切 ok。

> tips: 可以在生成的 URL 后加入高亮的语法来告诉 hastebin 如何渲染： `https://hastebin.purejs.icu/piqosotiyo.racket`。


## Conclusion
self-hosted 的意义也许就是有趣和随便造吧。
