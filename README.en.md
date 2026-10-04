<p align="center">
  <img src="./assets/profile-hero.svg" alt="pretextQ — Agents you can actually ship" width="100%" />
</p>

<p align="center">
  <a href="./README.md">简体中文</a>
  ·
  <a href="./README.zh-TW.md">繁體中文</a>
  ·
  <strong>English</strong>
</p>

<h1 align="center">Hi, I'm pretextQ 👋</h1>

<h3 align="center">I don't build chat demos — I build permission-controlled, verifiable systems that ship to enterprise production.</h3>

<p align="center">
  AI Agent Framework · Enterprise RBAC · Coding Agent Internals · API Test Automation · DevOps Engineering
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## Three repos, one engineering chain

I treat "can it ship to enterprise production" as the first metric of any system: the model is just the reasoning component — what actually decides success is the permission boundary, tool and context governance, verification tooling and observability.

These three projects cover the full engineering chain I care about:

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>⚙️ Agent Framework</strong><br/><br/>
      How an agent calls internal systems as the logged-in user, inside the enterprise permission model: unified entry point, RBAC inheritance, pluggable integrations, traces and audit.
    </td>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      How the core of a coding agent becomes a working implementation: agent loop, tool calling, permission layering, MCP extensions and context management.
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      How to prove system behavior is correct: YAML data-driven cases, dual response/database validation, continuous regression and CI.
    </td>
  </tr>
</table>

```text
atlas-claw              →  unified entry point, permission inheritance, pluggable integrations
MewCode                 →  a complete coding agent implementation: loop, tools, permissions
api-auto-test-framework →  prove every API behavior with data-driven dual validation
```

---

## 01 / atlas-claw

### [An enterprise Agent framework with a thin core](https://github.com/pretextQ/atlas-claw)

> Let the agent act as the logged-in user and call internal systems within the enterprise permission model — instead of scattering hardcoded integrations across the platform layer.

| Form | Focus | Status | Links |
|---|---|---|---|
| Enterprise AI Agent framework | Unified entry · RBAC inheritance · Pluggable providers | v0.1.0-alpha · active | [Repository](https://github.com/pretextQ/atlas-claw) |

atlas-claw is an AI Agent framework for enterprise scenarios: one unified conversational entry point across CRM, ITSM, monitoring, HR, finance and OA. The platform keeps a thin core — routing, auth and orchestration only; every integration is a pluggable provider.

### Making the agent actually land inside the intranet

```text
User (enterprise account / JWT)
      ↓
FastAPI service layer ── session · audit · trace persistence
      ↓
Agent orchestration (thin core)
      ↓
Pluggable providers
      ↓
CRM · ITSM · Monitoring · HR · Finance · OA
```

- **Unified conversational entry point**: one entry across internal systems; users never need to know which provider is behind.
- **Strict RBAC inheritance**: the conversation identity is the system identity — read-only users only see the systems and data they are allowed to.
- **Thin core + pluggable providers**: new integrations are standalone providers calling existing APIs directly, no hardcoded glue in the platform layer.
- **Embedded / standalone deployment**: embed into existing enterprise systems sharing users and org structure, or self-host standalone on the intranet.
- **Observable and auditable**: every provider call — params, latency, status code — persisted as traces, with audit reports and failure/latency analysis.
- **Designed for testing**: the core is not bound to any specific model; CI runs end-to-end regression against a fixed stub provider.

### The questions I care about in this project

The most common enterprise agent failures are not "not smart enough" — they are permission leaks, scattered integrations and untraceable behavior. atlas-claw puts all three at the design starting point: every model action stays inside the existing permission system, is traceable, and shows up in reports.

**Core Stack**

`Python` `FastAPI` `SQLAlchemy` `JWT / RBAC` `SQLite / MySQL / PostgreSQL` `Docker Compose` `GitHub Actions`

---

## 02 / MewCode

### [A complete AI Coding Agent implementation, from first principles](https://github.com/pretextQ/MewCode)

> The full anatomy of an AI coding agent — agent loop, tool calling, permission layering, MCP extensions — readable, runnable, modifiable.

| Form | Focus | Interface | Links |
|---|---|---|---|
| Complete AI coding agent implementation | Agent loop · Tool system · Permission layering | Textual terminal TUI | [Repository](https://github.com/pretextQ/MewCode) |

MewCode is a complete implementation of an AI coding agent, built to make the core principles and engineering practices concrete: not just "a loop that calls the model" — the tool system, permission model, context and memory management, and MCP extensions are all implemented end to end. The UI is built on Textual, and Windows is a first-class platform.

### The full skeleton of a coding agent

```text
User input (Textual TUI)
      ↓
Agent loop (asyncio-driven)
      ↓
LLM client (anthropic / openai / openai-compat protocols)
      ↓
Tool calls ── built-in tools · skills · MCP (stdio / Streamable HTTP)
      ↓
Permission layering ── sensitive actions require confirmation, never silent
      ↓
Context & memory management · hooks · file history
      ↓
Multi-agent collaboration (teams / worktrees)
```

- **Complete agent loop**: an asyncio-driven conversation loop with built-in Plan / AskUser / Permission dialogs — every step of the agent stays visible and controllable.
- **Multi-protocol model access**: anthropic / openai / openai-compat protocols work out of the box, with extended thinking support and API-key env-var fallback.
- **Extensible tool system**: built-in file and command tools, capabilities distilled into skills and memory, and MCP over stdio / Streamable HTTP for external tools.
- **Permission layering semantics**: sensitive actions raise a confirmation dialog instead of running silently; the permission semantics are documented with explicit boundaries.
- **Hook contract**: a clear stdin JSON contract — lifecycle hooks are observable and interceptable, so extension points behave predictably.
- **Multi-agent collaboration**: teams and worktrees isolate tasks, teammates are managed as a tree, and file history stays traceable.
- **Windows as a first-class citizen**: platform edges — GBK encoding, line endings, process-tree cleanup — are documented and handled.

### The questions I care about in this project

The internals of coding agents are locked inside commercial products: plenty of people use them, few can explain how the agent loop runs, how tool calls are orchestrated, or how permissions intercept actions. MewCode opens that black box — every step is readable, runnable, modifiable engineering code, not a concept diagram.

**Core Stack**

`Python 3.11` `Textual` `asyncio` `MCP` `uv` `pytest` `ruff` `mypy` `GitHub Actions`

---

## 03 / api-auto-test-framework

### [YAML-driven API testing with dual validation](https://github.com/pretextQ/api-auto-test-framework)

> YAML data-driven cases + dual validation turn API testing into a maintainable engineering asset — not one-off scripts.

| Form | Data-driven | Validation | Links |
|---|---|---|---|
| API test automation framework | YAML cases | JSONPath + SQL | [Repository](https://github.com/pretextQ/api-auto-test-framework) |

A Pytest-based API test automation framework for microservices: clone, install dependencies, run `pytest` — three steps, no extra setup scripts. A single YAML file covers single-API smoke tests, exception assertions and multi-step dependency chains.

### Turning API tests into a regression-ready asset

```text
YAML cases (single API / exceptions / chains)
      ↓
Pytest engine + chain context passing (extract → templates)
      ↓
JSONPath response assertions  +  SQL data checks
      ↓
Allure reports · Feishu notifications
      ↓
Dockerized runs · GitHub Actions CI
```

- **YAML data-driven**: cases read like documentation — smoke, exception and chain scenarios in one file.
- **Chain context passing**: extract + parameter templates make cross-API data flow read like a sentence.
- **JSONPath + SQL dual validation**: assert the response and the database state together, catching "200 OK but nothing persisted".
- **Multi-environment switching**: conftest-managed configs — one suite runs against dev, staging and test environments.
- **Closed engineering loop**: Allure reports, Feishu bot notifications, Dockerized runs and GitHub Actions CI.

### The questions I care about in this project

The value of automated testing is not how much it runs — it is whether failures pinpoint the cause and whether passes can be trusted. Dual validation and a clean layering make every assertion explainable and maintainable.

**Core Stack**

`Python` `Pytest` `YAML` `JSONPath` `SQL` `Allure` `Docker` `GitHub Actions` `Feishu`

---

## The method behind the three projects

| Principle | How I engineer |
|---|---|
| **Permissions first** | The agent acts as the logged-in user; RBAC decides what is visible and callable — unauthorized integrations never enter the platform layer. |
| **Evidence before answers** | Answers need grounding, assertions need data, findings need evidence — no conclusions that cannot be traced back. |
| **Thin core, replaceable parts** | Framework, retrieval and testing are all organized around a thin core; implementations are pluggable components. |
| **Observable and auditable** | Traces, audit reports and Allure reports come before feature piling — behavior can be replayed and measured. |
| **Everything into CI** | Stub regression, pytest + ruff + mypy checks and containerized runs all live in GitHub Actions; docs evolve with code. |

## Tech landscape

| Layer | Technologies | What I build |
|---|---|---|
| **Agent & Backend** | Python, FastAPI, SQLAlchemy, JWT / RBAC | Service layer, domain models, permission inheritance, session management |
| **Agent Internals** | Textual, asyncio, MCP, Hooks, uv | Agent loop, tool system, permission layering, context management |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, ruff, mypy | API regression, dual validation, static typing and lint checks |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, Feishu | CI, environment orchestration, reporting and notifications |

## Currently in progress

- Refining atlas-claw's embedded / standalone modes, expanding providers and audit reports
- Iterating on MewCode's multi-agent collaboration and skills ecosystem, hardening hooks and MCP edge cases
- Growing the validator and case library of api-auto-test-framework

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  If you are also working on enterprise agents, coding agent internals or test engineering — let's talk.
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
