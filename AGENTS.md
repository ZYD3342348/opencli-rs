# AGENTS.md

This repo has local design knowledge about parity with the TypeScript WeChat chatbot export flow.

## First Read

1. `docs/design/wechat-chatbot-export-parity.md`
2. `docs/design/local-knowledge-and-upstream-sync.md`

## Hard Rules

- `opencli-rs` 目前还没有完成 WeChat chatbot export 的正式实现。
- 在补这块能力之前，先以 `/Users/zengyuntan/python/opencli` 的真实验收结果为准，不要自行发明新默认链路。
- 当前已验证稳定链路来自 TypeScript 版：
  - `questionList`
  - `#app.__vue__` BFS
  - `batchDownload()`
  - 拦截 `/btsapi/v2/skill/export` 与 `/btsapi/v2/async/fetch`
- 不要默认采用：
  - FAQ 驱动
  - UI 点击
  - 坐标点击
  - synthetic event
- 真正的成功标准不是“拿到 URL”，而是“下载到非空 CSV”。

## When Implementing This Area

- 先读 TypeScript 版真实真相源
- 再写 parity 设计
- 再做实现
- 最后做真实浏览器验收

没有真实验收前，不要把任何新链路写成“稳定方案”。
