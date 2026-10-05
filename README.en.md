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
  AI Agent Framework · Enterprise RBAC · Coding Agent & Alert Automation · API Test Automation · DevOps Engineering
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
      How an AI coding agent turns production alerts into reviewable fix PRs unattended: OS-level sandbox, read-only toolchain, structured evidence chains and bounded retries.
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      How to prove system behavior is correct: YAML data-driven cases, dual response/database validation, continuous regression and CI.
    </td>
  </tr>
</table>

```text
atlas-claw              →  unified entry point, permission inheritance, pluggable integrations
MewCode                 →  alerts in, PRs out: unattended fixes in an OS sandbox
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

### [An AI Coding Agent that turns alerts into reviewed PRs](https://github.com/pretextQ/MewCode)

> One agent core, two shapes: an interactive CLI/TUI that writes code with you, and a headless service that stays resident on production alerts — the agent locates, fixes and verifies inside an OS-level sandbox, then hands the result back as a PR. Human PR review is the only mandatory gate; the service holds no merge permission.

| Form | Focus | Trust boundary | Links |
|---|---|---|---|
| AI Coding Agent + alert-driven dev service | Alert → fix PR · OS-level sandbox · bounded retries | Human review is the only gate · no merge permission | [Repository](https://github.com/pretextQ/MewCode) |

MewCode automates the on-call chain "alert arrives → locate → fix → verify → submit for review": Alertmanager webhooks trigger jobs, every job runs in its own sandbox and git worktree, and the PR body carries a structured evidence chain — alert summary, root cause, before/after test comparison, integration verification — so reviewers can judge without reading agent logs. A real-machine evaluation replay ran 6/6 jobs fully unattended: 100% fix-PR success rate, 25–26s median alert→PR.

### The unattended path from alert to PR

```text
Alert (Alertmanager webhook)
      ↓
Service layer ── SQLite state machine · fingerprint dedup · restart recovery
      ↓
Isolated unit per job (git worktree + Docker sandbox)
      ↓
Agent core ── locate · fix · bounded retries (escalate on limit)
      ↓
Verification ── regression tests + self-started docker-compose integration tests
      ↓
PR + CI gate ─▶ human review (the only release gate)
```

- **Alerts in, PRs out**: the demo repo keeps the real PRs from past unattended runs, each one a complete evidence chain — start from [PR #5](https://github.com/pretextQ/mewcode-alert-demo/pull/5).
- **Honest escalation**: retries are bounded; on limit the job escalates with the analysis it already produced. A fix that fails verification is never published — no garbage PRs.
- **OS-level sandbox**: the agent runs inside a container — non-root, all capabilities dropped, read-only root filesystem, CPU/memory/PID limits, hard-timeout kill; host config and secrets never enter the container.
- **Read-only internal toolchain**: two built-in MCP servers — production logs (Loki) and CI status (GitHub) — with GET-only data paths; tools without `readOnlyHint` are never mounted in service mode.
- **Self-started test environments**: when the repo ships a docker-compose.yml, the service brings up dependencies and runs integration tests inside the sandbox; with no container runtime it honestly records "not executed" instead of faking verification.
- **Measurable operations**: /metrics (Prometheus), per-job JSON retrospectives, per-repo token costs; evaluation-set replay gives success-rate, MTTR and cost before/after numbers for every prompt or core change.
- **Per-repo policy**: `.mewcode/policy.yaml` declares severity routing, target branch, token budget and notification channels — read per job, effective on save; broken policies fail fast instead of silently falling back.
- **The interactive kernel is complete too**: tool set + permission pipeline (deny overrides allowlist) + skills and MCP extensions; Windows is a first-class platform.

### The questions I care about in this project

The hard part of unattended coding is not fixing fast — it is what makes it trustworthy: how permissions are constrained, how behavior is isolated, how evidence is recorded, and how a broken fix is guaranteed never to ship. MewCode treats trust as the design starting point — in headless mode, actions that "would need to ask a human" are denied, not waved through; the PR is the only exit, the human is the only gate.

**Core Stack**

`Python 3.11` `Textual` `asyncio` `Docker sandbox` `MCP` `Prometheus` `SQLite` `GitHub Actions`

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
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent loop, permission layering, sandboxed execution, alert-driven service mode |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, ruff, mypy | API regression, dual validation, static typing and lint checks |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, Feishu | CI, environment orchestration, reporting and notifications |

## Currently in progress

- Refining atlas-claw's embedded / standalone modes, expanding providers and audit reports
- Hardening MewCode's sandbox egress whitelist and growing the alert evaluation suite
- Growing the validator and case library of api-auto-test-framework

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  If you are also working on enterprise agents, coding agent internals or test engineering — let's talk.
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
