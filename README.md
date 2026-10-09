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
  Coding Agent 与告警自动化 · 接口自动化测试 · DevOps 工程化
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

## 两个仓库，从自动修复到持续验证

我习惯把“能不能落进企业生产环境”当作系统的第一指标：模型只是其中的判断组件，真正决定成败的，是权限边界、工具与上下文治理、验证手段和可观测性。

这两个项目聚焦我最关心的自动修复与持续验证链路：

<table>
  <tr>
    <td width="50%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 如何无人值守地把线上告警变成可人审的修复 PR：OS 级沙箱、只读工具链、结构化证据链与有界重试。
    </td>
    <td width="50%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      如何证明系统行为正确：YAML 数据驱动、接口与数据库双重校验、持续回归与 CI 闭环。
    </td>
  </tr>
</table>

```text
MewCode                 →  告警进来、PR 出去：沙箱里的无人值守修复
api-auto-test-framework →  用数据驱动与双重校验证明每一次接口行为
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

## 02 / api-auto-test-framework

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

## 贯穿两个项目的工程原则

| 原则 | 我的工程取向 |
|---|---|
| **权限先行** | Agent 工具遵循权限分层与只读边界，修复在沙箱内执行，PR 经人工审核。 |
| **Evidence before answers** | 回答要有依据、断言要有数据、发现要有证据，不输出无法回溯的结论。 |
| **薄核心，可替换** | Agent 内核与测试都以薄核心组织，具体实现做成可插拔组件。 |
| **可观测与可审计** | 运行指标、任务复盘、Allure 报告先于功能堆叠，行为可回放、可统计。 |
| **一切进 CI** | 评估集回放、pytest + ruff + mypy 检查、容器化执行都进 GitHub Actions，文档与代码同步演进。 |

## 技术栈分层

| Layer | Technologies | What I build |
|---|---|---|
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent 内核、权限分层、沙箱执行与告警服务化 |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, ruff, mypy | 接口回归、双重校验、静态类型与风格检查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, 飞书 | CI、环境编排、报告与结果通知 |

## 最近在做的事

- 打磨 MewCode 的沙箱 egress 白名单与告警触达，扩充评估集用例
- 沉淀 api-auto-test-framework 的校验器与用例库

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企业 Agent 落地、Coding Agent 原理实现或测试工程化，欢迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
