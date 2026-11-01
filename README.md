<p align="center">
  <img src="./assets/profile-hero.svg" alt="pretextQ — Agents you can actually ship" width="100%" />
</p>

<p align="center">
  <strong>简体中文</strong>
  ·
  <a href="./README.zh-TW.md">繁體中文</a>
  ·
  <a href="./README.en.md">English</a>
</p>

<h1 align="center">你好，我是 pretextQ 👋</h1>

<h3 align="center">我构建的不是“会跑的 Demo”，而是权限可控、结果可验证、能落进企业生产环境的系统。</h3>

<p align="center">
  Agent 评估与执行观测 · Coding Agent 与告警自动化 · 长期记忆与知识工作台 · DevOps 工程化
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
  <!-- CSDN 和 Email 链接补好后取消注释:
  ·
  <a href="你的CSDN主页">CSDN</a>
  ·
  <a href="mailto:你的邮箱">Email</a>
  -->
</p>

---

## 三个仓库，覆盖 Agent 评估、执行与长期记忆

我习惯把“能不能落进企业生产环境”当作系统的第一指标：模型只是其中的判断组件，真正决定成败的，是权限边界、工具与上下文治理、验证手段和可观测性。

这三个项目分别聚焦 Agent 执行评估、自动修复与长期记忆：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>👁️ Agent Evaluation / Observation</strong><br/><br/>
      用外部测试集与自定义评分验证 Agent，关联执行轨迹、评分证据与版本回归报告。
    </td>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 如何无人值守地把线上告警变成可人审的修复 PR：OS 级沙箱、只读工具链、结构化证据链与有界重试。
    </td>
    <td width="33%" valign="top">
      <strong>🧠 Memory / Knowledge</strong><br/><br/>
      将笔记、文档与经历沉淀为长期记忆，通过混合检索、记忆治理与引用验证保留回答依据。
    </td>
  </tr>
</table>

```text
eyes                    →  Agent 测试、执行观测与有证据的回归对比
MewCode                 →  告警进来、PR 出去：沙箱里的无人值守修复
Reminder                →  长期记忆、混合检索与可追溯的知识工作台
```

---

## 01 / eyes

### [Agent evaluation and execution observation](https://github.com/pretextQ/eyes)

> 用外部测试集、自定义评分与执行证据，验证 Agent 修改是否有效，并看清每次任务如何执行。

| 项目形态 | 核心定位 | 当前状态 | Links |
|---|---|---|---|
| 自托管 Agent 测试与执行观测平台 | 并发实验 · 自定义评分 · 执行证据 · 回归对比 | v0.1.0 · 当前约定范围已交付，按需使用与维护 | [Repository](https://github.com/pretextQ/eyes) |

eyes 将目标 Agent、测试集版本、评分器版本和执行配置绑定到实验，关联用例执行、评分记录、轨迹与产物。支持 HTTP/Python 接入、独立评分进程、多 Agent 批次和持久化回归报告，也可通过 SDK 直接观测本地 Agent 会话，无需先创建实验。

### 从执行过程到回归证据

```text
目标 Agent + JSONL 测试集 + 自定义评分器
      ↓
控制后端与调度器 ── 固定版本 · 并发限制
      ↓
独立 Runner ── 执行尝试 · 轨迹 · 产物
      ↓
独立评分进程 ── 判定 · 理由 · 证据引用
      ↓
Web / CLI / API ── 回归报告 · CI 质量门槛
```

- **评分可追溯**：结论关联用例、执行尝试、评分口径和实际证据，区分执行失败、评分失败与证据不足。
- **执行过程可观测**：展示已采集的模型调用、工具执行和调用关系，明确采集覆盖范围与缺失状态。
- **回归口径明确**：固定实验配置，按用例版本比较改善与退化，单独标记测试集或评分口径变化带来的不可比项。
- **执行与评分分离**：独立 Runner 和评分进程，支持评分取消、重试及证据保留清理。
- **真实接入与运维验收**：MewCode Python/统一 HTTP、独立 trae HTTP、双 Agent 并发与定向恢复已有记录；trae 被动观测、离线补传、生命周期、容量告警与取消审计通过各自约定范围。
- **安全与资源边界加固**：10 月 22 日完成密钥引用校验、远程明文控制连接显式授权、取消派发竞态、事件与详情限额、证据恢复及本地保留清理等修复。静态检查、离线迁移 SQL、wheel 与 Web 构建通过；本轮真实并发、取消、配额与恢复仍待运行复验，见[维护记录](https://github.com/pretextQ/eyes/blob/main/docs/Review/2026-10-22-remediation.md)。
- **验证边界透明**：当前面向单团队自托管、按需运行；完整故障矩阵、长期容量与真实版本改善/退化尚未验收。编码专项首轮两题通过、订单题失败，后续重试为 unknown，无有效质量评分，见[真实评测记录](https://github.com/pretextQ/eyes/blob/main/docs/coding-regression-real-acceptance-20261016.md)。

**Core Stack**

`Python 3.14` `FastAPI` `SQLAlchemy` `PostgreSQL` `OpenTelemetry` `React` `TypeScript` `Vite`

---

## 02 / MewCode

### [An AI Coding Agent that turns alerts into reviewed PRs](https://github.com/pretextQ/MewCode)

> 交互式 CLI/TUI 与告警驱动服务共用 Agent 内核，在沙箱中完成定位、修复和验证，以 PR 交给人工审查。

| 项目形态 | 核心定位 | 信任边界 | Links |
|---|---|---|---|
| AI Coding Agent + 告警驱动的自动化开发服务 | 告警 → 修复 PR · OS 级沙箱 · 有界重试 | 人审 PR 唯一门禁 · 服务无 merge 权限 | [Repository](https://github.com/pretextQ/MewCode) |

### 从告警到 PR 的无人值守链路

```text
告警（Alertmanager webhook）
      ↓
服务层 ── SQLite 状态机 · 指纹去重 · 重启恢复
      ↓
每 job 隔离单元（git worktree + Docker 沙箱）
      ↓
Agent 内核 ── 定位 · 修复 · 有界重试（超限 escalate）
      ↓
验证 ── 回归测试 + 自起 docker-compose 集成测试
      ↓
PR + CI 门禁 ─▶ 人工 review（唯一上线门禁）
```

- **隔离与权限**：独立 worktree 与 Docker 沙箱、只读 MCP 工具链，执行受资源和权限边界约束。
- **有界修复**：自动串联定位、修复、回归与集成测试；重试超限升级人工，PR 附根因和测试证据。
- **可靠运行**：告警去重、重启恢复、通知 Outbox 与策略重验证，结合指标和 token 用量支持任务复盘。
- **扩展与审查**：支持 Skill/MCP 与 Windows；独立审查和监督默认关闭，仅提供意见与预算内反馈。
- **验证与边界**：已有[真实演示 PR](https://github.com/pretextQ/mewcode-alert-demo/pull/5)和[验收记录](https://github.com/pretextQ/MewCode/blob/master/docs/stage-2026-11-01-dead-end.md)；尚无生产收益结论，细粒度出网限制仍需宿主配置。

详细说明：[服务可靠性](https://github.com/pretextQ/MewCode/blob/master/guides/service-reliability.md) · [独立审查](https://github.com/pretextQ/MewCode/blob/master/guides/independent-review.md) · [监督指南](https://github.com/pretextQ/MewCode/blob/master/guides/supervision.md)

**Core Stack**

`Python 3.11` `Textual` `asyncio` `Docker 沙箱` `MCP` `Prometheus` `SQLite` `GitHub Actions`

---

## 03 / Reminder

### [The memory Agent at the heart of Mneme](https://github.com/pretextQ/Reminder)

> 将笔记、文档与经历沉淀为可检索、可追溯、可持续演化的个人记忆。

| 项目形态 | 核心定位 | 当前状态 | Links |
|---|---|---|---|
| Mneme 记忆 Agent 与知识工作台 | 混合检索 · 记忆治理 · 引用验证 · 可恢复执行 | v0.1.0 · 在线回答统一经过 Reminder API | [Repository](https://github.com/pretextQ/Reminder) |

Reminder 负责检索、记忆治理、回答生成与引用验证；Mneme 提供知识库、文档工作台、知识图谱、个人画像和成长分析。两者拥有独立数据库，通过版本化 HTTP 契约通信，明确数据所有权与服务边界。

### 让长期记忆有来源、有边界

```text
Vue 知识工作台 → Mneme API
      ↓
Reminder API → BGE-M3 + PostgreSQL / pgvector
      ↓
记忆治理 · 回答生成 · 引用验证
      ↓
耐久 Agent Run · Outbox / Inbox · 可重建投影
```

- **混合检索与证据引用**：融合语义向量、关键词与图谱信息，新增归一化后端分数融合与可选 BGE-M3 sparse 文档检索；稀疏向量未补齐时继续走 dense + keyword 路径。
- **知识库 MCP 接入**：新增指定飞书知识库与 Confluence Cloud 空间的只读目录、搜索及原文读取，见[飞书说明](https://github.com/pretextQ/Reminder/blob/master/docs/feishu-wiki-mcp.md)与[Confluence 说明](https://github.com/pretextQ/Reminder/blob/master/docs/confluence-mcp.md)。
- **记忆治理**：候选、人工治理、修订历史和证据关系共同维护长期记忆。
- **可恢复执行**：耐久运行记录、租约和幂等语义处理重试及进程中断。
- **删除与重建**：删除 fence 防止旧事件恢复已删数据，派生状态支持安全回填。
- **交付闭环**：Vue 工作台、Compose 服务栈、GHCR 版本镜像与 CI 评测一起维护。

**Core Stack**

`Python 3.12` `FastAPI` `Vue 3` `TypeScript` `PostgreSQL / pgvector` `BGE-M3` `Neo4j` `Redis / Celery` `Docker Compose`

---

## 贯穿三个项目的工程原则

| 原则 | 我的工程取向 |
|---|---|
| **权限先行** | Agent 工具遵循权限分层与只读边界，修复在沙箱内执行，PR 经人工审核。 |
| **Evidence before answers** | 回答要有依据、断言要有数据、发现要有证据，不输出无法回溯的结论。 |
| **薄核心，可替换** | Agent 内核与测试都以薄核心组织，具体实现做成可插拔组件。 |
| **可观测与可审计** | 运行指标、任务复盘、评分证据与引用审计先于功能堆叠，行为可回放、可统计。 |
| **一切进 CI** | 评估集回放、pytest + ruff + mypy 检查、容器化执行都进 GitHub Actions，文档与代码同步演进。 |

## 技术栈分层

| Layer | Technologies | What I build |
|---|---|---|
| **Agent Evaluation & Web** | Python, FastAPI, SQLAlchemy, PostgreSQL, OpenTelemetry, React, TypeScript | 服务控制面、独立 Runner、执行轨迹、评分与回归报告 |
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent 内核、权限分层、沙箱执行与告警服务化 |
| **Memory & Knowledge** | Vue, FastAPI, PostgreSQL / pgvector, BGE-M3, Neo4j, Redis / Celery | 长期记忆、知识工作台、图谱投影与可恢复异步执行 |
| **Testing & Quality** | Pytest, ruff, mypy, TypeScript | Agent 评测、回归验证、静态类型与风格检查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, GHCR, Prometheus | CI、环境编排、版本镜像、监控与运维 |

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企业 Agent 落地、Coding Agent 原理实现、Agent 评估或长期记忆系统，欢迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
