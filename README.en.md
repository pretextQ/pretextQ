<p align="center">
  <img src="./assets/profile-hero.svg" alt="pretextQ — Framework, Knowledge, Verification" width="100%" />
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
  AI Agent Framework · Enterprise RBAC · RAG · Retrieval Evaluation · API Test Automation · DevOps Engineering
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## Three repos, one engineering chain

I treat "can it ship to enterprise production" as the first metric of any system: the model is just the reasoning component — what actually decides success is the permission boundary, data ownership, retrieval quality, verification tooling and observability.

These three projects cover the full engineering chain I care about:

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>⚙️ Agent Framework</strong><br/><br/>
      How an agent calls internal systems as the logged-in user, inside the enterprise permission model: unified entry point, RBAC inheritance, pluggable integrations, traces and audit.
    </td>
    <td width="33%" valign="top">
      <strong>🧠 Knowledge / RAG</strong><br/><br/>
      How documents become verifiable knowledge: multimodal parsing, hybrid retrieval, citation checking, and measurable quality evaluation.
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      How to prove system behavior is correct: YAML data-driven cases, dual response/database validation, continuous regression and CI.
    </td>
  </tr>
</table>

```text
atlas-claw              →  unified entry point, permission inheritance, pluggable integrations
GraphScholarV1          →  documents into verifiable knowledge: hybrid retrieval and citations
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

## 02 / GraphScholarV1

### [A multimodal RAG knowledge base with verifiable answers](https://github.com/pretextQ/GraphScholarV1)

> Turn documents into retrievable, citable knowledge — every answer should be able to point back to its source.

| Form | Focus | Evaluation | Links |
|---|---|---|---|
| Full-stack RAG application | Multimodal knowledge-base QA | Built-in RAGAS | [Repository](https://github.com/pretextQ/GraphScholarV1) |

GraphScholarV1 is a multimodal RAG knowledge-base QA system for personal knowledge management: upload documents → intelligent parsing → hybrid indexing → precise QA. Separated frontend/backend: FastAPI + SQLAlchemy services, React 18 + Vite + Tailwind workbench.

### A retrieval pipeline that pins answers to their sources

```text
Upload (PDF / Markdown / scanned / images)
      ↓
Parse · chunk · OCR / VLM image captions
      ↓
BM25 sparse recall  +  ChromaDB dense recall
      ↓
Query rewrite → routing → Rerank → citation check
      ↓
Answer + cited sources (auto second-pass on low confidence)
```

- **Multimodal document parsing**: PDF / Markdown / plain text, OCR for scanned pages, VLM captions for images — all retrievable.
- **Hybrid retrieval**: BM25 sparse recall + dense vector recall in parallel, covering both keyword hits and semantic similarity.
- **Full retrieval pipeline**: query rewrite, question routing, Rerank refinement, citation checking — low confidence automatically triggers a second retrieval pass.
- **Incremental indexing**: file hash fingerprints detect changes and re-index automatically, no service restart.
- **Layered caching**: exact QA match → semantic vector cache → normal retrieval; a hit skips the whole pipeline.
- **Measurable quality**: built-in RAGAS evaluation module — retrieval and generation quality come with numbers, not vibes.

### The questions I care about in this project

The hard part of RAG is not storing vectors — it is whether the answer stays faithful to the documents: can every sentence point back to a citation, how does the system prove itself when retrieval fails, and how are quality changes measured. GraphScholarV1 treats citation checking and RAGAS evaluation as first-class citizens.

**Core Stack**

`Python 3.14` `FastAPI` `React 18` `PostgreSQL` `ChromaDB` `BM25` `Reranker` `RAGAS` `LangChain`

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
| **Evidence before answers** | Answers need citations, assertions need data, findings need evidence — no conclusions that cannot be traced back. |
| **Thin core, replaceable parts** | Framework, retrieval and testing are all organized around a thin core; implementations are pluggable components. |
| **Observable and auditable** | Traces, audit reports and Allure reports come before feature piling — behavior can be replayed and measured. |
| **Everything into CI** | Stub regression, RAGAS evaluation and containerized runs all live in GitHub Actions; docs evolve with code. |

## Tech landscape

| Layer | Technologies | What I build |
|---|---|---|
| **Agent & Backend** | Python, FastAPI, SQLAlchemy, JWT / RBAC | Service layer, domain models, permission inheritance, session management |
| **RAG & Data** | BM25, ChromaDB, Reranker, PostgreSQL, MySQL | Hybrid retrieval, citation checking, caching and index governance |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, RAGAS | API regression, dual validation, retrieval quality evaluation |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, Feishu | CI, environment orchestration, reporting and notifications |

## Currently in progress

- Refining atlas-claw's embedded / standalone modes, expanding providers and audit reports
- RAGAS-driven retrieval quality improvements and multimodal parsing for GraphScholarV1
- Growing the validator and case library of api-auto-test-framework
- Ongoing experiments in Lyra4DAgent · AgentKit · MewCode

---

<h3 align="center">Build the agent. Ground it in facts. Prove it works.</h3>

<p align="center">
  If you are also working on enterprise agents, RAG quality or test engineering — let's talk.
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
