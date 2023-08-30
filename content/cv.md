+++
title = "jinser's CV"
path = "cv"
+++

# 锦瑟 <sub><font color="#808B96">Jinser Kafka</font></sub>

cmdr.jv@gmail.com | [github.com/jetjinser](https://github.com/jetjinser) | [purejs.icu](https://www.purejs.icu)

对技术有热情，保持学习冲动。


## 教育经历
- [2022-] 计算机科学与技术，<!-- 仰恩 -->*大学，本科大一在读


## 实习经历

### `Second State` 后端开发：flows.network

[flows.network](https://github.com/flows-network/) 是一个使用 [WasmEdge](https://wasmedge.org) 作为运行时的 serverless 平台。

#### server 开发
开发语言和框架：
- `rust/axum`
- `typescript/next.js`

持久化：
- `upstash(redis)`
- `postgresql`

对接各平台，根据平台文档实现代码，为 flows.network 提供鉴权和监听能力。一般用的手段为 webhook server，websocket server 或 http post 轮训，如 BlockNative server 作为 webhook server 监听地址事件。

对 server 开发更熟练。

e.g.
- [Github server](https://github.com/flows-network/github-flows/tree/master/github-integration)
- [Telegram server](https://github.com/flows-network/tg-flows/tree/master/tg-integration)

#### sdk 开发
为 flows.network 提供第三方平台 action 能力。

根据第三方平台的文档，或借助社区生态，使用 rust 编写 http sdk，target 为 wasm32-wasi。如 Github 的 create issue, merge pr 等。

对 wasm 中的网络环境与请求掌握更熟练。

e.g.
- [Notion sdk](https://github.com/flows-network/notion-flows/tree/master/sdk)
- [Discord sdk](https://github.com/flows-network/discord-flows/tree/master/sdk)


##### codegen
实现 sdk 时常需要 codegen。利用 rust 的宏能力和生态（`syn`，`quote`），编译时解析 spec（e.g. Github openapi）文件，展开为对应的 Event struct, http method...加深了对 rust procmacro 的理解。

#### 魔改 rust 库，使其支持 wasm32-wasi
移植 rust 库到 wasm32-wasi。

主要的缺乏为 https(ssl/tls) 方面。

加深对 wasm 开发与加密基本库的认知。

e.g.
- [notion-wasi](https://github.com/flows-network/notion-wasi)
- [Firebase-rs](https://github.com/jetjinser/firebase-rs/tree/diff-client)


## 开源贡献

### WasmEdge
#### openssl plugin
这个 [PR](https://github.com/WasmEdge/WasmEdge/pull/2425)
解决了描述在 openssl/openssl#7147 的问题，这个问题导致的结果是，在缺少 hostname 检验的时候，会发生 SSL connection 错误。

通过设置 openssl hostname 来解决这个问题，修复了插件对于部分链接无法请求的错误。

学习到在 WasmEdge 中如何通过 host function 来编写插件。


### RisingLight
#### div-0 error
这个 [PR](https://github.com/risinglightdb/risinglight/pull/793) 解决了描述在[risinglight/risinglight#773](https://github.com/risinglightdb/risinglight/issues/773) 的问题，即除 0 错误。

在 ArrayImpl div 之前将被除数安全化，使被标记为 null 的项都重设为 1 而不是 0。这样既解决了除 0 问题，也保留了自动向量化的能力。


### RisingWave
#### fix array_agg
[PR](https://github.com/risingwavelabs/risingwave/pull/6084):
fix(expr, agg): batch array_agg return NULL when there's no input raws

## 专业技能

### lang
- 熟悉 Rust
- 熟悉 Python
- 了解 Haskell

### tech
- 了解基本的 database 知识
- 了解基本的 static analysis 知识


## 兴趣

- programming language theory
- category theory
- RISC-V
- typesetting system
