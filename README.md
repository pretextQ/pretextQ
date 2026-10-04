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
  AI Agent Framework · Enterprise RBAC · Coding Agent 原理与实现 · 接口自动化测试 · DevOps 工程化
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

## 三个仓库，一条完整的工程链路

我习惯把“能不能落进企业生产环境”当作系统的第一指标：模型只是其中的判断组件，真正决定成败的，是权限边界、工具与上下文治理、验证手段和可观测性。

这三个项目恰好覆盖了我最关心的完整工程链路：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>⚙️ Agent Framework</strong><br/><br/>
      Agent 如何以登录用户的身份、在企业权限体系内调用内部系统：统一入口、权限继承、可插拔集成、Trace 与审计。
    </td>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 的核心原理如何落地为工程实现：Agent 主循环、工具调用、权限分层、MCP 扩展与上下文管理。
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      如何证明系统行为正确：YAML 数据驱动、接口与数据库双重校验、持续回归与 CI 闭环。
    </td>
  </tr>
</table>

```text
atlas-claw              →  企业 Agent 的统一入口、权限继承与可插拔集成
MewCode                 →  完整实现 AI Coding Agent：主循环、工具与权限
api-auto-test-framework →  用数据驱动与双重校验证明每一次接口行为
```

---

## 01 / atlas-claw

### [An enterprise Agent framework with a thin core](https://github.com/pretextQ/atlas-claw)

> 让 Agent 以登录用户的身份、在企业权限体系内调用内部系统完成任务——而不是在平台层散落硬编码集成。

| 项目形态 | 核心定位 | 当前状态 | Links |
|---|---|---|---|
| 企业级 AI Agent 框架 | 统一入口 · RBAC 继承 · 可插拔 Provider | v0.1.0-alpha · 活跃开发 | [Repository](https://github.com/pretextQ/atlas-claw) |

atlas-claw 是面向企业场景的 AI Agent 框架：用一个统一对话入口打通 CRM、ITSM、监控、HR、财务、OA 等内部系统。平台层保持薄核心，只负责路由、鉴权与编排；具体系统接入全部做成可插拔 Provider。

### 让 Agent 真正落进企业内网

```text
用户（企业账号 / JWT）
      ↓
FastAPI 服务层 ── 会话 · 审计 · Trace 落库
      ↓
Agent 编排（薄核心）
      ↓
可插拔 Provider
      ↓
CRM · ITSM · 监控 · HR · 财务 · OA
```

- **统一对话入口**：一个入口串联多个内部系统，用户不需要知道背后接了哪个 Provider。
- **严格继承 RBAC 权限**：对话身份即系统身份，只读用户只能看见自己有权限的系统与数据。
- **薄核心 + 可插拔 Provider**：新系统接入写成独立 Provider，内部直调既有接口，不在平台层堆积硬编码集成。
- **内嵌 / 独立两种部署**：既可嵌进企业现有系统共享用户与组织架构，也可独立部署在内网自托管。
- **可观测与可审计**：每次 Provider 调用的参数、耗时、状态码落库成 Trace，附审计报表、失败率与耗时分析。
- **为测试而设计**：核心不绑定具体模型，CI 中以固定 Stub Provider 跑端到端回归。

### 这个项目真正要解决的问题

Agent 在企业里最常见的失败不是“不够聪明”，而是权限失控、集成散落、行为无法追溯。atlas-claw 把这三件事放在设计起点：让模型的每一次行动都在既有权限体系内、有 Trace 可查、可被报表统计。

**Core Stack**

`Python` `FastAPI` `SQLAlchemy` `JWT / RBAC` `SQLite / MySQL / PostgreSQL` `Docker Compose` `GitHub Actions`

---

## 02 / MewCode

### [A complete AI Coding Agent implementation, from first principles](https://github.com/pretextQ/MewCode)

> 把 AI Coding Agent 的完整实现摊开来看：Agent 主循环、工具调用、权限分层、MCP 扩展——可读、可跑、可改。

| 项目形态 | 核心定位 | 界面形态 | Links |
|---|---|---|---|
| AI Coding Agent 完整实现 | Agent 原理 · 工具系统 · 权限分层 | Textual 终端 TUI | [Repository](https://github.com/pretextQ/MewCode) |

MewCode 是一个 AI Coding Agent 的完整实现，目标是把 AI Agent 的核心原理与工程实践讲清楚：不止是“会调模型的循环”，而是把工具系统、权限模型、上下文与记忆管理、MCP 扩展一一实现到位。界面基于 Textual 构建，Windows 是一等公民运行平台。

### 一个 Coding Agent 的完整骨架

```text
用户输入（Textual TUI）
      ↓
Agent 主循环（asyncio 驱动）
      ↓
LLM 客户端（anthropic / openai / openai-compat 三协议）
      ↓
工具调用 ── 内建工具 · Skills · MCP（stdio / Streamable HTTP）
      ↓
权限分层 ── 敏感操作弹窗确认，不静默执行
      ↓
上下文与记忆管理 · Hooks · 文件历史
      ↓
多 Agent 协作（teams / worktree）
```

- **完整 Agent 主循环**：asyncio 驱动的对话循环，Plan / AskUser / Permission 对话框内建，Agent 每一步行为可见、可控。
- **多协议模型接入**：anthropic / openai / openai-compat 三种协议即配即用，支持 extended thinking，API Key 走环境变量回退。
- **可扩展的工具系统**：内建文件与命令工具，Skills 与 Memory 沉淀能力，MCP 以 stdio / Streamable HTTP 双传输接入外部工具。
- **权限分层语义**：敏感操作弹出确认对话框而不是静默执行，权限语义写成文档，危险动作有明确边界。
- **Hooks 契约**：stdin JSON 契约清晰，生命周期钩子可观测、可拦截，扩展点行为可预期。
- **多 Agent 协作**：teams 与 worktree 做任务隔离，teammate 树状管理，文件历史可回溯。
- **Windows 一等公民**：GBK 编码读取、行尾保持、进程树清理等平台边界有专门文档与处理。

### 这个项目真正要解决的问题

AI Coding Agent 的内部实现大多封装在商业产品里：会用的人多，能说清主循环怎么转、工具怎么编排、权限怎么拦截的人少。MewCode 把这层黑盒打开——每一步都是可读、可跑、可改的工程代码，而不是概念图。

**Core Stack**

`Python 3.11` `Textual` `asyncio` `MCP` `uv` `pytest` `ruff` `mypy` `GitHub Actions`

---

## 03 / api-auto-test-framework

### [YAML-driven API testing with dual validation](https://github.com/pretextQ/api-auto-test-framework)

> 用 YAML 数据驱动 + 双重校验，把接口测试做成可以持续回归的工程资产，而不是一次性脚本。

| 项目形态 | 数据驱动 | 校验方式 | Links |
|---|---|---|---|
| 接口自动化测试框架 | YAML 用例 | JSONPath + SQL | [Repository](https://github.com/pretextQ/api-auto-test-framework) |

基于 Pytest 的微服务接口自动化测试框架：克隆、装依赖、`pytest` 三步跑通，不需要额外配置脚本。单 YAML 文件即可覆盖单接口冒烟、异常断言与多步依赖链路。

### 把接口测试做成可回归的工程资产

```text
YAML 用例（单接口 / 异常 / 链路）
      ↓
Pytest 执行引擎 + 上下文传参（extract → 模板）
      ↓
JSONPath 响应断言  +  SQL 数据校验
      ↓
Allure 报告 · 飞书结果通知
      ↓
Docker 化执行 · GitHub Actions CI
```

- **YAML 数据驱动**：用例即文档，单文件覆盖冒烟、异常与多步链路场景。
- **链路上下文传参**：extract 提取 + 参数模板，跨接口取数像读句子一样自然。
- **JSONPath + SQL 双重校验**：接口响应与数据库状态同时断言，防住“返回 200 但数据没落库”这类问题。
- **多环境切换**：conftest 管理环境配置，一套用例跑遍开发、测试、预发。
- **结果工程闭环**：Allure 报告、飞书机器人通知、Docker 化执行、GitHub Actions CI 全部就位。

### 这个项目真正要解决的问题

自动化测试的价值不在跑得多，而在失败时能否直接定位、成功时能否信任。响应与数据库的双重校验、清晰的分层结构，让每一次断言都可解释、可维护。

**Core Stack**

`Python` `Pytest` `YAML` `JSONPath` `SQL` `Allure` `Docker` `GitHub Actions` `飞书`

---

## 贯穿三个项目的工程原则

| 原则 | 我的工程取向 |
|---|---|
| **权限先行** | Agent 以登录用户身份行动，RBAC 决定能看见什么、调用什么，未授权的集成不进平台层。 |
| **Evidence before answers** | 回答要有依据、断言要有数据、发现要有证据，不输出无法回溯的结论。 |
| **薄核心，可替换** | 框架、Agent 内核、测试都以薄核心组织，具体实现做成可插拔组件。 |
| **可观测与可审计** | Trace、审计报表、Allure 报告先于功能堆叠，行为可回放、可统计。 |
| **一切进 CI** | Stub 回归、pytest + ruff + mypy 检查、容器化执行都进 GitHub Actions，文档与代码同步演进。 |

## 技术栈分层

| Layer | Technologies | What I build |
|---|---|---|
| **Agent & Backend** | Python, FastAPI, SQLAlchemy, JWT / RBAC | 服务层、领域模型、权限继承与会话管理 |
| **Agent Internals** | Textual, asyncio, MCP, Hooks, uv | Agent 主循环、工具系统、权限分层与上下文管理 |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, ruff, mypy | 接口回归、双重校验、静态类型与风格检查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, 飞书 | CI、环境编排、报告与结果通知 |

## 最近在做的事

- 打磨 atlas-claw 的内嵌 / 独立双模式，扩充内部系统的 Provider 与审计报表
- 迭代 MewCode 的多 Agent 协作与 Skills 生态，补齐 Hooks 与 MCP 的边界场景
- 沉淀 api-auto-test-framework 的校验器与用例库
- Lyra4DAgent · AgentKit 等实验仓库的持续迭代

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企业 Agent 落地、Coding Agent 原理实现或测试工程化，欢迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
