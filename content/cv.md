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

[flows.network](https://github.com/flows-network/)
是一个使用 Wasmedge 作为运行时的 serverless 平台。

#### server 开发
开发语言和框架：
- `rust/axum`
- `typescript/next.js`

持久化：
- `upstash(redis)`
- `postgresql`

对接各平台，根据平台文档实现代码，为 flows.network 提供鉴权和监听能力。

#### sdk 开发
为 flows.network 提供第三方平台 action 能力。

根据第三方平台的文档，使用 rust 编写 http sdk，
target 为 wasm32-wasi。

#### codegen
实现 sdk 时常需要 codegen。
利用 rust 的宏能力和生态（`syn`，`quote`），
编译时解析 spec（e.g. Github openapi）文件，
展开为对应的 Event struct, http method...

#### 魔改 rust 库，使其支持 wasm32-wasi
魔改 rust 第三方库，使其支持 wasm32-wasi。

主要的缺乏为 https(ssl/tls) 方面。


## 开源贡献

### Wasmedge
#### openssl plugin
这个 [PR](https://github.com/WasmEdge/WasmEdge/pull/2425)
解决了描述在 openssl/openssl#7147 的问题，这个问题
导致的结果是，在缺少 hostname 检验的时候，会发生 SSL connection 错误。

通过设置 openssl hostname 来解决这个问题，
修复了插件对于部分链接无法请求的错误。


### Risinglight
#### div-0 error
这个 [PR](https://github.com/risinglightdb/risinglight/pull/793)
解决了描述在
[risinglight/risinglight#773](https://github.com/risinglightdb/risinglight/issues/773)
的问题，即除 0 错误。

在 ArrayImpl div 之前将被除数安全化，使被标记为 null 的项都重设为 1 而不是 0。
这样既解决了除0问题，也保留了自动向量化的能力。


### RisingWave
#### fix array_agg
[PR](https://github.com/risingwavelabs/risingwave/pull/6084):
fix(expr, agg): batch array_agg return NULL when there's no input raws

## 专业技能

### lang
- Rust
- Python
- Haskell

### tech
- database
- static analysis
