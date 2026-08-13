---
name: dsh-harness-specs
description: Hermes Harness 控制平面规格（MVP 与受监督 Worker）的解读与应用。 / Expert for the Hermes Harness control-plane specifications: MVP v1 and supervised Worker v1.
---

# Hermes Harness 规格文档专家 / Hermes Harness Specification Expert

本技能面向 Hermes Harness 控制平面规格：`specs/Harness-MVP-v1.md`（需求编译为 TaskContract、权威注册表、有界 Loop、可恢复账本）与 `specs/Harness-Worker-v1.md`（受监督闭环：材料快照、人工批准启动、确定性验证、修复一次、人工事实复核）。规格为纯文档；设计或评审 Hermes 风格的任务控制平面时以其为事实依据。

This skill covers the Hermes Harness control-plane specifications: `specs/Harness-MVP-v1.md` (compile requirements into TaskContracts, authoritative registry, bounded loops, recoverable ledger) and `specs/Harness-Worker-v1.md` (supervised closed loop: material snapshots, human-approved start, deterministic verification, fix-once, human fact review). Pure documentation; treat them as the source of truth when designing or reviewing Hermes-style task control planes.

## When to use / 何时使用

设计需求到任务的编译 / 路由 / 执行框架；需要可恢复账本与有界 Loop；需要受监督 Worker（人工批准启动、只读执行、确定性验证与引文核验）；需要评审「不扩大权限」的业务闭环。

When designing requirement-to-task compilation/routing/execution frameworks; recoverable ledgers and bounded loops; supervised workers with human-approved start, read-only execution and deterministic verification; or reviewing privilege-minimal business closed loops.

## Workflow / 工作流

1. 先读 MVP 规格，掌握公共接口（注册表镜像 / New-Task / Loop / Ledger）与三种需求分流。
2. 再读受监督 Worker 规格，理解输入边界、执行与验证、人工事实复核。
3. 用「验收标准」核对实现：监督模式默认不调用模型、Loop 最多两次、终态不重复派发。

1. Read the MVP spec first: public interfaces (registry mirror / New-Task / Loop / Ledger) and the three-way request routing.
2. Then the Supervised Worker spec: input boundaries, execution/verification and human fact review.
3. Check implementations against the acceptance criteria: supervised mode never calls a model by default, loops run at most twice, terminal states are never re-dispatched.

## References / 参考

- 项目 README: 见仓库根目录
- 作者: h565656445 (GitHub)