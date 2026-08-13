# dsh-harness-specs

<!-- DeepSeek Harness 衍生声明 -->
> **DeepSeek Harness 个人适配声明（Personal Adaptation Notice）**
>
> 本项目是 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 的**个人适配产物（personal adaptation）**，**并非 DeepSeek Harness 官方文件（not an official DeepSeek Harness file）**，随附功能、使用说明与个人产物（bundled with features, documentation, and personal artifacts），可与 DeepSeek Harness 搭配使用，也可独立使用。
>
> This project is a **personal adaptation** for DeepSeek Harness, and is **NOT an official DeepSeek Harness file**, bundled with features, documentation, and personal artifacts. It can be used alongside DeepSeek Harness or standalone.

**作者 / Author**: [h565656445](https://github.com/h565656445)

**合作 / Collaboration**: 如有项目可以一起合作，欢迎联系。微信：`wohaishihenshuaide`。If you have projects, let's collaborate. WeChat: `wohaishihenshuaide`.


---

## 用途 / What this is for

Harness 核心规格：MVP v1 与受监督 Worker v1，描述控制平面如何生成合同、受监督执行与状态闭环。

Harness core specs: MVP v1 and supervised Worker v1.

---
## Hermes Harness Specifications / Hermes Harness 规格文档

本仓库收录 Hermes Harness 控制平面的两份核心规格：MVP v1 与受监督 Worker v1。MVP 规格定义「自然语言需求 → 可校验 TaskContract → 有界执行 Loop → 可恢复账本」的编译与执行框架；受监督 Worker 规格定义在不扩大权限前提下的业务闭环：人工批准启动、确定性验证、修复一次、人工事实复核。本仓库为纯文档仓库，不包含任何源码。

This repository collects the two core control-plane specifications of Hermes Harness: MVP v1 and Supervised Worker v1. The MVP spec defines the framework that compiles natural-language requirements into verifiable TaskContracts and runs them through bounded loops with a recoverable ledger; the Supervised Worker spec defines a business closed loop without privilege expansion — human-approved start, deterministic verification, fix-once, and human fact review. Pure documentation; no source code is included.

## Features / 功能

- **MVP v1**：需求编译为 TaskContract；明确 / 模糊 / 敏感需求分别进入 routed / waiting_for_user / waiting_for_approval
  MVP v1: compile requirements into TaskContracts; routed / waiting_for_user / waiting_for_approval for clear, ambiguous and sensitive requests
- **权威注册表**：Markdown 为唯一事实源，机器运行镜像可溯源到路径与 SHA-256
  Authoritative registry: Markdown as single source of truth, machine mirror traceable by path and SHA-256
- **有界 Loop**：最多执行两次、只允许一次修正，随后 completed 或 failed
  Bounded loop: at most two runs, one fix allowed, then completed or failed
- **受监督 Worker v1**：材料快照与哈希、人工批准启动、只读执行、确定性 Verifier 与 source_quote 引文核验
  Supervised Worker v1: material snapshots with hashes, human-approved start, read-only execution, deterministic verifier with source_quote checks
- **可恢复账本**：JSONL 事件可重建状态、终态不重复派发；全程凭据扫描、不自动发布
  Recoverable ledger: JSONL events rebuild state, terminal states never re-dispatched; credential scanning, no auto-publish

## What's inside / 目录结构

```
dsh-harness-specs/
├── README.md              # 双语说明 + DSH 衍生声明
├── LICENSE                # MIT
├── specs/                 # Hermes Harness 控制平面规格（2 份，纯文档）
│   ├── Harness-MVP-v1.md
│   └── Harness-Worker-v1.md
└── .dsh/                  # DeepSeek Harness 衍生包
    ├── preset.yml
    ├── agent.cordis.yml
    ├── README.md
    └── skills/dsh-harness-specs/SKILL.md
```

## Quick start / 快速开始

```powershell
# 1. 浏览规格清单
$repo = "E:\path\to\dsh-harness-specs"
Get-ChildItem (Join-Path $repo "specs") -Filter *.md | Select-Object Name

# 2. 阅读 MVP 规格
Get-Content (Join-Path $repo "specs\Harness-MVP-v1.md")

# 3. 安装 DSH 预设（可选）
$dst = Join-Path $env:DSH_HOME ".agent-presets\harness-specs"
Copy-Item -Recurse -Force (Join-Path $repo ".dsh") $dst
```

## DeepSeek Harness 衍生 / DSH Derivative

本项目附带 DeepSeek Harness 衍生包，位于 `.dsh/` 目录：

- `preset.yml` — Agent 预设元数据
- `agent.cordis.yml` — Cordis 组装（基于 standard 预设，persona 已定制）
- `skills/dsh-harness-specs/SKILL.md` — 项目专属技能（skill）

安装与接入方式见 [`.dsh/README.md`](.dsh/README.md)（双语）。


## License / 许可证

[MIT](LICENSE)

---

---

## 相关项目 / Related Projects

> 这是 DeepSeek Harness 个人适配系列（共 31 个仓库）的完整导航。 / This is the complete navigation for the DeepSeek Harness personal-adaptation series (31 repos).

### Agent OS 内核 / Kernel

[`dsh-agent-os-runtime`](https://github.com/h565656445/dsh-agent-os-runtime) · [`dsh-agent-os-planning`](https://github.com/h565656445/dsh-agent-os-planning) · [`dsh-agent-os-scheduler`](https://github.com/h565656445/dsh-agent-os-scheduler) · [`dsh-agent-os-worker-protocol`](https://github.com/h565656445/dsh-agent-os-worker-protocol) · [`dsh-agent-os-observability`](https://github.com/h565656445/dsh-agent-os-observability) · [`dsh-agent-os-specs`](https://github.com/h565656445/dsh-agent-os-specs)

### Harness 基础设施 / Infrastructure

[`dsh-harness-core`](https://github.com/h565656445/dsh-harness-core) · [`dsh-graph-entry`](https://github.com/h565656445/dsh-graph-entry) · [`dsh-async-job`](https://github.com/h565656445/dsh-async-job) · [`dsh-file-identity`](https://github.com/h565656445/dsh-file-identity) · [`dsh-json-projection`](https://github.com/h565656445/dsh-json-projection) · [`dsh-manual-approval`](https://github.com/h565656445/dsh-manual-approval) · [`dsh-observation-writer`](https://github.com/h565656445/dsh-observation-writer) · [`dsh-provider-control`](https://github.com/h565656445/dsh-provider-control) · [`dsh-schema-negotiator`](https://github.com/h565656445/dsh-schema-negotiator) · [`dsh-upgrade-governance`](https://github.com/h565656445/dsh-upgrade-governance)

### 规格与文档 / Specs & Docs

**`dsh-harness-specs`（本仓库 / this repo）** · [`dsh-novel-specs`](https://github.com/h565656445/dsh-novel-specs) · [`dsh-architecture-guide`](https://github.com/h565656445/dsh-architecture-guide) · [`dsh-powershell-patterns`](https://github.com/h565656445/dsh-powershell-patterns) · [`dsh-json-schema-driven-dev`](https://github.com/h565656445/dsh-json-schema-driven-dev) · [`dsh-llm-agent-harness-guide`](https://github.com/h565656445/dsh-llm-agent-harness-guide)

### 适配器 / Adapters

[`dsh-short-story-engine`](https://github.com/h565656445/dsh-short-story-engine) · [`dsh-tutorial-video-state-machine`](https://github.com/h565656445/dsh-tutorial-video-state-machine) · [`dsh-governance-kernel`](https://github.com/h565656445/dsh-governance-kernel) · [`dsh-sports-pipeline`](https://github.com/h565656445/dsh-sports-pipeline) · [`dsh-motion-grammar`](https://github.com/h565656445/dsh-motion-grammar)

### DSH 总集成 / Integration

[`dsh-integration`](https://github.com/h565656445/dsh-integration) · [`dsh-presets-pack`](https://github.com/h565656445/dsh-presets-pack) · [`dsh-skills-pack`](https://github.com/h565656445/dsh-skills-pack) · [`dsh-starter-kit`](https://github.com/h565656445/dsh-starter-kit)

