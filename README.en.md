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
  Coding Agent & Alert Automation · Agent Evaluation & Observation · Long-term Memory & Knowledge Workspace · DevOps Engineering
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## Three repos: agent execution, evaluation and long-term memory

I treat "can it ship to enterprise production" as the first metric of any system: the model is just the reasoning component — what actually decides success is the permission boundary, tool and context governance, verification tooling and observability.

These three projects focus on automated agent fixes, execution evaluation and long-term memory:

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      How an AI coding agent turns production alerts into reviewable fix PRs unattended: OS-level sandbox, read-only toolchain, structured evidence chains and bounded retries.
    </td>
    <td width="33%" valign="top">
      <strong>👁️ Agent Evaluation / Observation</strong><br/><br/>
      Evaluate agents with external datasets and custom scoring, connecting traces and evidence to versioned regression reports.
    </td>
    <td width="33%" valign="top">
      <strong>🧠 Memory / Knowledge</strong><br/><br/>
      Build lasting memory from notes and documents with hybrid retrieval, memory governance and verifiable citations.
    </td>
  </tr>
</table>

```text
MewCode                 →  alerts in, PRs out: unattended fixes in an OS sandbox
eyes                    →  agent evaluation, execution observation and regression evidence
Reminder                →  long-term memory, hybrid retrieval and traceable knowledge
```

---

## 01 / MewCode

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

## 02 / eyes

### [Agent evaluation and execution observation](https://github.com/pretextQ/eyes)

> Evaluate agent changes with external datasets, custom scoring and execution evidence.

| Form | Focus | Status | Links |
|---|---|---|---|
| Self-hosted agent evaluation and observation platform | Concurrent experiments · scoring · traces · regression reports | Backend, Runner, SDK and console implemented; acceptance work remains | [Repository](https://github.com/pretextQ/eyes) |

eyes connects versioned experiments to execution attempts, scores, traces and artifacts. It supports HTTP/Python integration, multi-agent batches and passive observation of local agent sessions.

### From execution to regression evidence

```text
Agent + JSONL dataset + custom scorer
      ↓
Control backend / scheduler ── versions · concurrency limits
      ↓
Independent Runner ── attempts · traces · artifacts
      ↓
Scoring process ── decisions · reasons · evidence references
      ↓
Web / CLI / API ── regression reports · CI gates
```

- **Traceable scores**: link decisions to attempts, scoring rules and evidence.
- **Visible observation gaps**: show collected calls and relationships alongside missing evidence.
- **Comparable regressions**: pin configurations and flag dataset or scoring changes.
- **Separate scoring**: independent execution and scoring, with cancellation, retries and retention controls.
- **Explicit validation scope**: Deta/Zeta Python concurrency and cancellation validated; real HTTP agents, full Runner restart recovery and capacity acceptance remain pending.

**Core Stack**

`Python 3.14` `FastAPI` `SQLAlchemy` `PostgreSQL` `OpenTelemetry` `React` `TypeScript` `Vite`

---

## 03 / Reminder

### [The memory Agent at the heart of Mneme](https://github.com/pretextQ/Reminder)

> Turn notes, documents and experiences into searchable, traceable personal memory.

| Form | Focus | Status | Links |
|---|---|---|---|
| Mneme memory agent and knowledge workspace | Retrieval · memory governance · citations · recovery | v0.1.0 · online answers routed through Reminder API | [Repository](https://github.com/pretextQ/Reminder) |

Reminder handles retrieval, memory governance, answers and citation verification. Mneme supplies the workspace, knowledge graph, profiles and growth analysis. Separate databases and versioned HTTP contracts define their ownership boundaries.

### Memory with evidence and clear boundaries

```text
Vue workspace → Mneme API
      ↓
Reminder API → BGE-M3 + PostgreSQL / pgvector
      ↓
Memory governance · answers · citation verification
      ↓
Durable Agent Run · Outbox / Inbox · rebuildable projections
```

- **Grounded retrieval**: combine vectors, keywords and graph information with answer evidence.
- **Governed memory**: candidates, human review, revisions and evidence relationships.
- **Recoverable runs**: durable records, leases and idempotency support retries and interruptions.
- **Deletion boundaries**: deletion fences prevent stale events from restoring removed data.
- **Delivery workflow**: workspace, Compose services, versioned GHCR images and CI evaluations.

**Core Stack**

`Python 3.12` `FastAPI` `Vue 3` `TypeScript` `PostgreSQL / pgvector` `BGE-M3` `Neo4j` `Redis / Celery` `Docker Compose`

---

## The method behind the three projects

| Principle | How I engineer |
|---|---|
| **Permissions first** | Agent tools follow permission layers and read-only boundaries; fixes run in sandboxes and PRs receive human review. |
| **Evidence before answers** | Answers need grounding, assertions need data, findings need evidence — no conclusions that cannot be traced back. |
| **Thin core, replaceable parts** | The agent core and testing are organized around a thin core; implementations are pluggable components. |
| **Observable and auditable** | Runtime metrics, job reports, scoring evidence and citation audits come before feature piling — behavior can be replayed and measured. |
| **Everything into CI** | Evaluation replays, pytest + ruff + mypy checks and containerized runs all live in GitHub Actions; docs evolve with code. |

## Tech landscape

| Layer | Technologies | What I build |
|---|---|---|
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent loop, permission layering, sandboxed execution, alert-driven service mode |
| **Agent Evaluation & Web** | Python, FastAPI, SQLAlchemy, PostgreSQL, OpenTelemetry, React, TypeScript | Control plane, independent Runner, traces, scoring and regression reports |
| **Memory & Knowledge** | Vue, FastAPI, PostgreSQL / pgvector, BGE-M3, Neo4j, Redis / Celery | Long-term memory, knowledge workspace, graph projections and recoverable workflows |
| **Testing & Quality** | Pytest, ruff, mypy, TypeScript | Agent evaluations, regression verification, static typing and lint checks |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, GHCR, Prometheus | CI, orchestration, versioned images, monitoring and operations |

## Currently in progress

- Hardening MewCode's sandbox egress whitelist and growing the alert evaluation suite
- Improving eyes' real-agent integrations and regression evidence, with recovery, cancellation and capacity acceptance checks
- Refining Reminder's context governance and compaction safeguards, strengthening memory evidence traceability and regression evaluations

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  If you are also working on enterprise agents, coding agent internals, agent evaluation or long-term memory systems — let's talk.
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
