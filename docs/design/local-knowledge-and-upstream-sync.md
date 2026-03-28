# Local Knowledge And Upstream Sync

> 最后更新：2026-03-28
> 适用仓库：`/Users/zengyuntan/python/opencli-rs`

## 先看结论

`opencli-rs` 这边现在最容易丢的，不是代码，而是“我们已经在 TypeScript 版验证过哪些坑、哪些链路是真的”。

所以这里要保留的，不只是实现计划，还包括一套固定的接管入口。

当前方案是：

1. `AGENTS.md` 负责把 AI 先带到正确文档
2. `docs/design/wechat-chatbot-export-parity.md` 负责写清楚要对齐什么
3. 这份文档负责写清楚以后怎么跟上游同步，而不把本地经验冲掉

## 为什么 `opencli-rs` 更需要这一层

因为它现在还没真正实现 WeChat chatbot export。

这意味着以后谁先来补，很容易出现两种问题：

- 从零重新试错，重复走 UI 点击和 FAQ 的弯路
- 只看上游代码结构，没有把 TypeScript 版真实验收经验一起带过来

所以这里必须先把“经验入口”固定住。

## 推荐工作方式

以后如果要在 `opencli-rs` 里推进这块，建议固定按这个顺序：

1. 先读 `AGENTS.md`
2. 再读 `docs/design/wechat-chatbot-export-parity.md`
3. 再去对照 `/Users/zengyuntan/python/opencli/docs/wechat-chatbot-export-source-of-truth.md`
4. 确认 TypeScript 真相源有没有变化
5. 再开始写 Rust 方案或实现

## 和上游同步时怎么保住经验

推荐策略：

- 把本地知识放在低冲突文档里，而不是散落在聊天记录里
- 把上游更新合进自己的工作分支，而不是直接覆盖本地判断
- 一旦上游未来也做了相似能力，先核对真实验收证据，再决定是否切换口径

## 什么时候更新这些文档

出现下面这些变化时，就该更新：

- TypeScript 真相源更新了
- Rust 版开始正式实现了
- Rust 版做了真实浏览器验收
- 发现之前记录的某个判断已经失效

## 一条判断原则

以后无论是人还是 AI，如果在这块遇到“有新实现，但不知道该不该信”，先看两件事：

1. 有没有真实页面证据
2. 有没有非空 CSV 证据

没有这两类证据，就先别把它写成新的稳定方案。
