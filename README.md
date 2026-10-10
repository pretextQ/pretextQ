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
  Coding Agent 与告警自动化 · Agent 评估与执行观测 · 长期记忆与知识工作台 · DevOps 工程化
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

## 三个仓库，覆盖 Agent 执行、评估与长期记忆

我习惯把“能不能落进企业生产环境”当作系统的第一指标：模型只是其中的判断组件，真正决定成败的，是权限边界、工具与上下文治理、验证手段和可观测性。

这三个项目分别聚焦 Agent 自动修复、执行评估与长期记忆：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 如何无人值守地把线上告警变成可人审的修复 PR：OS 级沙箱、只读工具链、结构化证据链与有界重试。
    </td>
    <td width="33%" valign="top">
      <strong>👁️ Agent Evaluation / Observation</strong><br/><br/>
      用外部测试集与自定义评分验证 Agent，关联执行轨迹、评分证据与版本回归报告。
    </td>
    <td width="33%" valign="top">
      <strong>🧠 Memory / Knowledge</strong><br/><br/>
      将笔记、文档与经历沉淀为长期记忆，通过混合检索、记忆治理与引用验证保留回答依据。
    </td>
  </tr>
</table>

```text
MewCode                 →  告警进来、PR 出去：沙箱里的无人值守修复
eyes                    →  Agent 测试、执行观测与有证据的回归对比
Reminder                →  长期记忆、混合检索与可追溯的知识工作台
```

---

## 01 / MewCode

### [An AI Coding Agent that turns alerts into reviewed PRs](https://github.com/pretextQ/MewCode)

> 同一个 agent 内核，两种形态：交互式 CLI/TUI 帮你写代码；无头服务常驻接线上告警——agent 在 OS 级沙箱里定位、修复、验证，以 PR 交给人工 review。人审 PR 是唯一必经的人工点，服务没有任何 merge 权限。

| 项目形态 | 核心定位 | 信任边界 | Links |
|---|---|---|---|
| AI Coding Agent + 告警驱动的自动化开发服务 | 告警 → 修复 PR · OS 级沙箱 · 有界重试 | 人审 PR 唯一门禁 · 服务无 merge 权限 | [Repository](https://github.com/pretextQ/MewCode) |

MewCode 把“收到告警 → 定位 → 修复 → 验证 → 提交 review”这条值班链路自动化：Alertmanager webhook 触发，每个 job 在独立沙箱与 git worktree 中执行，PR body 自带结构化证据链——告警摘要、根因、测试前后对比、集成验证——review 者不看 agent 日志也能做判断。真机评估集回放 6 单全部无人值守跑通：修复 PR 成功率 100%，告警→PR 中位 25–26 秒。

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

- **告警进来，PR 出去**：demo 仓库保留历次无人值守运行的真实 PR，每份都是完整证据链，可从 [PR #5](https://github.com/pretextQ/mewcode-alert-demo/pull/5) 看起。
- **修不好就诚实升级**：重试有上限，超限进入 escalate 并附上已尝试的分析；验证不过的修复不会被发布，绝不产生垃圾 PR。
- **OS 级沙箱**：agent 在容器内执行——非 root、全部能力丢弃、只读根文件系统、CPU/内存/PID 限额、硬超时强杀，宿主配置与密钥不进容器。
- **内部工具链只读**：内置生产日志（Loki）与 CI 状态（GitHub）两个 MCP server，整条取数路径只有 GET；未声明 readOnlyHint 的工具在服务模式一律不挂。
- **自起测试环境**：仓库带 docker-compose.yml 时自动起依赖，在沙箱内跑集成测试；无容器运行时则如实记录“未执行”，不用假验证充数。
- **运营可量化**：/metrics（Prometheus）、单 job JSON 复盘、按仓库 token 成本；评估集回放让每次提示词/内核改动都有成功率、MTTR 与成本的前后对比。
- **多仓库策略**：`.mewcode/policy.yaml` 声明触发路由、目标分支、token 预算与通知渠道，按 job 现读、改文件即生效，损坏的策略快速失败而非静默回退。
- **交互式内核同样完整**：工具集 + 权限分层管线（deny 优先于白名单）+ Skill 与 MCP 扩展，Windows 是一等公民平台。

### 这个项目真正要解决的问题

无人值守写代码最难的不是“修得快”，而是“凭什么信”：权限怎么收敛、行为怎么隔离、证据怎么沉淀、改坏了怎么保证不上线。MewCode 把信任当成设计起点——headless 模式下“要问人”的动作一律拒绝而非放行，PR 是唯一出口，人是唯一门禁。

**Core Stack**

`Python 3.11` `Textual` `asyncio` `Docker 沙箱` `MCP` `Prometheus` `SQLite` `GitHub Actions`

---

## 02 / eyes

### [Agent evaluation and execution observation](https://github.com/pretextQ/eyes)

> 用外部测试集、自定义评分与执行证据，验证 Agent 修改是否有效，并看清每次任务如何执行。

| 项目形态 | 核心定位 | 当前状态 | Links |
|---|---|---|---|
| 自托管 Agent 测试与执行观测平台 | 并发实验 · 自定义评分 · 执行证据 · 回归对比 | 控制后端、Runner、SDK 与 Web 控制台已实现；部分验收待完成 | [Repository](https://github.com/pretextQ/eyes) |

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
- **验证边界透明**：已完成 Deta、Zeta 两个真实 Python Agent 的并发与取消验证；真实 HTTP Agent、Runner 整体重启恢复及容量验收仍待完成。

**Core Stack**

`Python 3.14` `FastAPI` `SQLAlchemy` `PostgreSQL` `OpenTelemetry` `React` `TypeScript` `Vite`

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

- **混合检索与证据引用**：融合语义向量、关键词与图谱信息，保留回答依据。
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
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent 内核、权限分层、沙箱执行与告警服务化 |
| **Agent Evaluation & Web** | Python, FastAPI, SQLAlchemy, PostgreSQL, OpenTelemetry, React, TypeScript | 服务控制面、独立 Runner、执行轨迹、评分与回归报告 |
| **Memory & Knowledge** | Vue, FastAPI, PostgreSQL / pgvector, BGE-M3, Neo4j, Redis / Celery | 长期记忆、知识工作台、图谱投影与可恢复异步执行 |
| **Testing & Quality** | Pytest, ruff, mypy, TypeScript | Agent 评测、回归验证、静态类型与风格检查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, GHCR, Prometheus | CI、环境编排、版本镜像、监控与运维 |

## 最近在做的事

- 打磨 MewCode 的沙箱 egress 白名单与告警触达，扩充评估集用例
- 完善 eyes 的真实 Agent 接入与回归证据，推进故障恢复、取消和容量验收
- 打磨 Reminder 的上下文治理与压缩保护，完善长期记忆的证据追溯与回归评测

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企业 Agent 落地、Coding Agent 原理实现、Agent 评估或长期记忆系统，欢迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
