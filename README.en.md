# pretextQ

Building verifiable, observable Agent systems with explicit permission boundaries.

<a href="./README.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <strong>English</strong>

## Selected projects

<table width="100%">
<tr><td width="9999">
<h3><img src="./assets/eyes.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/eyes">eyes</a></h3>
<p><strong>Agent evaluation and execution observation</strong><br/>
Evaluate Agent changes with your own datasets, scoring rules and execution evidence.</p>
<p>Versioned experiments · Independent scoring · Traces · Regression reports<br/>
<sub>Python / FastAPI / React / PostgreSQL</sub></p>
<p><a href="https://github.com/pretextQ/eyes">Repository</a> · <a href="https://github.com/pretextQ/eyes#两条使用路径">Integration &amp; usage</a><br/>
<sub>v0.1.0 · Self-hosted for a single team; on-demand use and maintenance</sub></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><img src="./assets/mewcode.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/MewCode">MewCode</a></h3>
<p><strong>From production alerts to reviewable repair PRs</strong><br/>
Investigate, fix and verify in isolated worktrees and Docker sandboxes, then hand the PR to a human.</p>
<p>Bounded retries · Read-only MCP · Notification Outbox · Independent review<br/>
<sub>Python / Textual / Docker / SQLite</sub></p>
<p><a href="https://github.com/pretextQ/MewCode">Repository</a> · <a href="https://github.com/pretextQ/mewcode-alert-demo/pull/5">Real repair PR</a><br/>
<sub>v0.3.0 · Shared Agent core for CLI / TUI and alert services; no merge permissions</sub></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><img src="./assets/reminder.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/Reminder">Reminder</a></h3>
<p><strong>Long-term memory with sources and governance</strong><br/>
Turn notes, documents and experiences into searchable, traceable personal memory for the Mneme knowledge workspace.</p>
<p>Hybrid retrieval · Memory governance · Citation checks · Recoverable runs<br/>
<sub>Python / FastAPI / Vue / pgvector / BGE-M3</sub></p>
<p><a href="https://github.com/pretextQ/Reminder">Repository</a> · <a href="https://github.com/pretextQ/Reminder/blob/master/docs/architecture.md">Architecture &amp; boundaries</a><br/>
<sub>v0.1.0 · Online answers routed through the Reminder API</sub></p>
</td></tr>
</table>

## Recent progress

- **2026-11-01 · MewCode** — Delivered notification Outbox, policy revalidation on recovery and feedback for repeated pytest failures. Independent review and supervision are off by default; model effectiveness and production benefits remain unverified. [Delivery records](https://github.com/pretextQ/MewCode/blob/master/docs/README.md)

- **2026-10-22 · eyes** — Security and resource fixes passed static checks and builds; new runtime acceptance still needs verification. Experience improvements remain in the backlog. [Maintenance](https://github.com/pretextQ/eyes/blob/main/docs/Review/2026-10-22-remediation.md) · [Design backlog](https://github.com/pretextQ/eyes/blob/main/docs/TODO.md)

- **Reminder · On-demand maintenance** — Hybrid score fusion, optional BGE-M3 sparse retrieval and read-only Feishu / Confluence MCP connectors are available. [Feishu](https://github.com/pretextQ/Reminder/blob/master/docs/feishu-wiki-mcp.md) · [Confluence](https://github.com/pretextQ/Reminder/blob/master/docs/confluence-mcp.md)

## Technical focus

Agent Runtime · Evaluation & Observability · Memory & Retrieval · Infrastructure

Explicit permissions, bounded execution and traceable evidence. Implementation, real-world verification and planned work are recorded separately.

---

<sub>Status checked: 2026-11-01 (Asia/Shanghai) · eyes c94d500 / MewCode cc98162 / Reminder 9f1d5e8</sub>
