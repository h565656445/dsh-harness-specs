---
type: spec
status: active
created: 2026-07-17
updated: 2026-07-17
---

# Hermes Harness 受监督 Worker v1

## 目标

在不扩大权限的前提下，把首条业务切片接成真实闭环：

```text
自然语言需求 → TaskContract → 剪辑项目的运动切片 → Worker准备
→ 人工批准启动 → Execute → Verify → Fix once → Ledger
```

## 公共入口

1. `harness_runner.ps1 -ExecutionMode Supervised`：生成 TaskContract、材料快照、项目上下文快照和可检查的 `worker_prompt.txt`，返回 `awaiting_worker_start`，不调用模型。
2. `codex_worker.ps1 -ContractPath ... -ApproveExecution`：从同一任务恢复，调用本机 `codex exec` 并进入有界 Loop。
3. `Invoke-HarnessCodexTask`：供 Runner 调用的公共接口；测试可注入替代调用器，生产默认使用本机 Codex CLI。

## 输入边界

- 当前只执行 `ai_content` 路由中的运动内容草稿，且该路由必须携带 `sports + create` 证据；明确AI生成时可在同一路由增加 `ai_video` 证据，但不因此授权本Worker执行视频生成切片。
- 执行前至少提供一份本地文本材料；允许 `.txt`、`.md`、`.json`、`.yaml`、`.yml`、`.csv`、`.srt`，单文件不超过 1MB，最多8份。
- 材料在任务目录按原始字节快照并记录 SHA-256；疑似密码、令牌、Cookie、API Key 或云访问密钥在创建任务目录前阻断。
- `provided` 表示用户提供；`verified` 只表示调用方已确认核验状态，Harness 不凭标签自行完成事实核验。

## 执行与验证

- Codex 在单个任务目录内以 `read-only`、`ephemeral` 和结构化输出 Schema 运行；模型工具没有任务目录写权限，最终 JSON 只由 Codex 主进程写到 Harness 指定路径。
- Worker 只能读取合同、材料快照与项目规则/状态快照，只能生成候选草稿。
- 准备阶段把合同、原始输入、材料、项目上下文、提示和结果 Schema 组成 `worker_package.json`，批准前后逐项复算哈希；变化即阻断。
- 结果必须声明实际使用的材料哈希、预计时长、四项安全自检，并为事实主张提供能在对应材料中逐字命中的 `source_quote`。
- 确定性 Verifier 拒绝未知/变化材料哈希、伪造引文、未核验主张、超时、发布尝试、权威文件修改或阻塞项；引文命中只能证明证据存在，语义是否充分仍保留人工验收。
- 第一次失败只允许携带验证原因修复一次；第二次仍失败进入 `failed`。
- 结构验证通过时先持久化 `result.txt` 与 `execution_receipt.json`，成功后才把合同和 Ledger 提交为 `waiting_for_approval/content_fact_review`；候选写入失败不得发布待复核状态，也不能自动成为 `completed`。

## 非目标

- 不自动发布，不创建正式视频母版，不修改体育项目四份生产真相。
- 不接跨项目任务图，不创建新 Agent，不做多模型调度。
- 不把一次执行经验自动写入长期规则。
- 不把模型自报的安全检查当作事实核验本身；它只作为结构化验证输入。

## 验收

- 默认监督模式不会调用模型；批准后可从同一 task 恢复。
- 主 Runner 只能准备，不能在首次调用跳过可见暂停点；执行只能由恢复入口显式批准。
- 输入、合同、输出、验证与状态都可定位并带哈希或路径证据。
- Worker 包 SHA-256 锚定在只追加 Ledger，清单或受保护文件变化都会阻断。
- Loop 最多两次，失败关闭；结构通过后等待人工事实复核，终态或中断态不能重复派发。

返回：[[10-项目/Hermes-Harness/README|Hermes Harness]]。
