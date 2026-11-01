# pretextQ

Building verifiable, observable Agent systems with explicit permission boundaries.

<a href="./README.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <strong>English</strong>

## Selected projects

<table width="100%">
<tr><td width="9999">
<h3><picture><source media="(max-width: 600px)" srcset="./assets/flow-spacer.svg" /><img align="right" width="38%" src="./assets/flow-eyes.svg" alt="Dataset to Agent to scoring, linked to evidence and regression reports" /></picture><img src="./assets/eyes.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/eyes">eyes</a></h3>
<p><strong>Agent evaluation and execution observation</strong><br/>
Self-hosted evaluation and observation, backed by scores and execution evidence.<br/>
<sub>v0.1.0 · Python / FastAPI / React / PostgreSQL</sub><br/>
<a href="https://github.com/pretextQ/eyes">Repository</a> · <a href="https://github.com/pretextQ/eyes#两条使用路径">Integration &amp; usage</a></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><picture><source media="(max-width: 600px)" srcset="./assets/flow-spacer.svg" /><img align="right" width="38%" src="./assets/flow-mewcode.svg" alt="Alert to sandboxed repair to pull request, followed by human review" /></picture><img src="./assets/mewcode.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/MewCode">MewCode</a></h3>
<p><strong>From production alerts to reviewable repair PRs</strong><br/>
Fix and verify in a sandbox; hand the PR to a human. No merge permissions.<br/>
<sub>v0.3.0 · Python / Textual / Docker / SQLite</sub><br/>
<a href="https://github.com/pretextQ/MewCode">Repository</a> · <a href="https://github.com/pretextQ/mewcode-alert-demo/pull/5">Real repair PR</a></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><picture><source media="(max-width: 600px)" srcset="./assets/flow-spacer.svg" /><img align="right" width="38%" src="./assets/flow-reminder.svg" alt="Sources to governed memory to retrieval, grounding answers in citations" /></picture><img src="./assets/reminder.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/Reminder">Reminder</a></h3>
<p><strong>Long-term memory with sources and governance</strong><br/>
Searchable, traceable personal memory for the Mneme knowledge workspace.<br/>
<sub>v0.1.0 · Python / FastAPI / Vue / pgvector / BGE-M3</sub><br/>
<a href="https://github.com/pretextQ/Reminder">Repository</a> · <a href="https://github.com/pretextQ/Reminder/blob/master/docs/architecture.md">Architecture &amp; boundaries</a></p>
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
