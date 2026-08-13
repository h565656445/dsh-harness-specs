---
type: spec
status: active
created: 2026-07-17
updated: 2026-07-17
---

# Hermes Harness MVP v1

## 目标

把自然语言需求编译为可校验的 TaskContract，依据权威项目注册表选择现有项目，通过有界 Loop 执行或暂停，并把状态写入可恢复账本。

## 公共接口

1. `Update-HarnessProjectRegistry`：从 `00-系统/项目注册表.md` 生成机器运行镜像；Markdown 始终是唯一事实源。
2. `New-HarnessTask`：把原始需求解析为 TaskContract；明确需求进入 `routed`，模糊需求进入 `waiting_for_user`，敏感副作用进入 `waiting_for_approval`。
3. `Invoke-HarnessTaskLoop`：最多执行两次，第一次验证失败只允许修正一次，随后必须 `completed` 或 `failed`。
4. Ledger：每次状态变化追加 JSONL 事件，任务中断后可恢复最后状态。

## MVP 范围

- 只接入本地项目注册表和 Codex 运行入口。
- 第一条业务纵向切片是“基于已提供或已核验资料生成运动内容草稿”。
- 默认只路由和创建任务合同；可选受监督模式只接入带材料的运动内容草稿，不自动发布，不自动修改网站正式页面。
- 项目保持独立，不移动、不合并。

## 非目标

- 不自动修改 Harness 核心规则。
- 不自动创建新 Agent。
- 不做多模型智能调度。
- 不安装 LangGraph、Microsoft Agent Framework 或复杂 RAG。
- 不把一次任务经验自动晋级长期记忆。

## 验收标准

- 机器注册表可追溯到权威 Markdown 的路径与 SHA-256。
- 明确、模糊和敏感操作需求得到不同且可解释的状态。
- TaskContract 通过 JSON Schema 校验。
- Loop 不会无限运行，状态变化可从 Ledger 重建。
- 新增 Markdown 从 `[[首页]]` 可达，图谱审计保持零断链、零不可达、零孤岛。

## 依据

- 任务前规划系统 2026-07-14（链接失效）
- [[00-系统/项目注册表]]
- [[00-系统/工作流决策]]
- [[10-项目/Hermes-Harness/specs/Harness-Worker-v1|受监督 Worker v1]]
