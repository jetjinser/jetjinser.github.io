# 锦瑟

cmdr.jv@gmail.com | [github.com/jetjinser](https://github.com/jetjinser) | [purejs.icu](https://www.purejs.icu)

对技术有热情，保持学习冲动


## 教育经历
- [2022-] 计算机科学与技术，仰恩大学，本科大一在读


## 实习经历

### `Second State` 后端开发：flows.network

flows.network 是一个使用 Wasmedge 作为运行时的 serverless 平台。

#### server 开发
针对不同的平台实现不同的 server，我们称其为 `integration`。
主要的功能为对接 flows.network 与各平台。

如 Github integration，为 flows.network 提供监听 Github 事件的能力；
Slack integration，为 flows.network 提供监听消息和事件的能力。

#### sdk 开发
在 flows.network 中，既有 integration 提供的监听能力，还需要有 action 的能力，
sdk 即为在 wasm32-wasi 中提供 action 能力的开发套件。

我使用 rust 编写，target 为 wasm32-wasi。

#### codegen
曾使用过 codegen 的技术编写 Github sdk。
利用 rust 的宏能力和生态（`syn`，`quote`），
编译时解析 Github openapi spec 文件，展开为对应的 Event struct, http method...

#### 魔改 rust 库，使其支持 wasm32-wasi
wasm32-wasi 的生态并不完善，许多库在开发和维护时并不考虑该 target。
在 Second State 实习期间，我魔改了不少第三方库，使它们支持 wasm32-wasi。


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


## 专业技能

### lang
- Rust
- Python
- Haskell

### tech
- database
- static analysis
