# WeChat Chatbot Export Parity

> 最后更新：2026-03-28
> 适用仓库：`/Users/zengyuntan/python/opencli-rs`

## 先看结论

`opencli-rs` 目前还没有对齐 TypeScript 版的 WeChat chatbot export 能力。

所以这里先落的是“对齐目标”和“不要走错的边界”，不是假装已经做完实现。

当前应该对齐的真实真相源在：

- `/Users/zengyuntan/python/opencli/docs/wechat-chatbot-export-source-of-truth.md`

## 需要对齐的稳定链路

TypeScript 版已经做过真实验证，当前稳定链路是：

1. 进入 `questionList`
2. 从 `#app.__vue__` 做 Vue 组件 BFS
3. 找到同时具备 `batchDownload()` 和 `downLoadFile()` 的组件
4. 直接调用 `vm.batchDownload()`
5. 拦截并解析：
   - `POST /btsapi/v2/skill/export`
   - `POST /btsapi/v2/async/fetch`
6. 最终下载 CSV，并确认非空

`opencli-rs` 以后如果补这块，应该以这条链路为第一目标，而不是重新从 UI 点击开始试错。

## 为什么不要从别的方案开始

### FAQ 不是当前默认目标

原因不是 FAQ 永远不可能用，而是现阶段没有足够稳定证据：

- 真实页面里 FAQ 组件并不稳定可见
- 之前出现过 FAQ 导出只拿到表头的情况

### UI 点击不是 CLI 正式方案

原因很直接：

- 合成事件会被页面识别成 `isTrusted=false`
- 坐标点击太脆，不适合长期维护
- 一旦页面布局轻微变化，CLI 就会误点

### 裸 fetch 不是当前正确入口

因为已经验证过会出现 `invalid sign`，说明签名和上下文不适合脱离页面内部逻辑去硬拼。

## `opencli-rs` 的实现建议

未来真正实现这块时，建议按下面顺序设计：

1. 保留与 TypeScript 类似的“页面内执行 + 拦截解析”思路
2. 在 Rust 侧把“页面内脚本模板”和“拦截结果解析”分开
3. 把“组件发现失败”“导出接口未发起”“拿到 URL 但 CSV 为空”分别做成不同错误
4. 把“非空 CSV”写进验收和测试口径

## 最重要的验收标准

未来 `opencli-rs` 补齐这块时，不要只验：

- 命令返回成功
- 拿到 task_id
- 拿到 download_url

必须再加一层：

- 下载 CSV
- 确认不只是表头

## 这份文档的定位

这不是实现文档，而是“防止未来重走弯路”的设计边界文档。

它的作用是让新来的 AI 或开发者先知道：

- 该抄哪条真相源
- 哪些坑已经踩过
- 哪些方案现在不能当默认方案
