# pretextQ

建構可驗證、可觀測、權限可控的 Agent 系統。

<a href="./README.md">简体中文</a> · <strong>繁體中文</strong> · <a href="./README.en.md">English</a>

## 精選專案

<table width="100%">
<tr><td width="9999">
<h3><picture><source media="(max-width: 600px)" srcset="./assets/flow-spacer.svg" /><img align="right" width="38%" src="./assets/flow-eyes.svg" alt="測試集到 Agent 執行與評分，再關聯證據和迴歸報告" /></picture><img src="./assets/eyes.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/eyes">eyes</a></h3>
<p><strong>Agent 評測與執行觀測</strong><br/>
自託管評測與觀測，讓 Agent 修改有評分、有證據。<br/>
<sub>v0.1.0 · Python / FastAPI / React / PostgreSQL</sub><br/>
<a href="https://github.com/pretextQ/eyes">儲存庫</a> · <a href="https://github.com/pretextQ/eyes#两条使用路径">接入與使用</a></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><picture><source media="(max-width: 600px)" srcset="./assets/flow-spacer.svg" /><img align="right" width="38%" src="./assets/flow-mewcode.svg" alt="告警到沙箱修復，再產生 PR 並交給人工審查" /></picture><img src="./assets/mewcode.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/MewCode">MewCode</a></h3>
<p><strong>從線上告警到可審查的修復 PR</strong><br/>
在沙箱中修復、驗證，以 PR 交給人工；服務無合併權限。<br/>
<sub>v0.3.0 · Python / Textual / Docker / SQLite</sub><br/>
<a href="https://github.com/pretextQ/MewCode">儲存庫</a> · <a href="https://github.com/pretextQ/mewcode-alert-demo/pull/5">真實修復 PR</a></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><picture><source media="(max-width: 600px)" srcset="./assets/flow-spacer.svg" /><img align="right" width="38%" src="./assets/flow-reminder.svg" alt="來源到受治理的記憶與檢索，再產生有引用的回答" /></picture><img src="./assets/reminder.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/Reminder">Reminder</a></h3>
<p><strong>有來源、可治理的長期記憶</strong><br/>
為 Mneme 知識工作台提供可檢索、可追溯的個人記憶。<br/>
<sub>v0.1.0 · Python / FastAPI / Vue / pgvector / BGE-M3</sub><br/>
<a href="https://github.com/pretextQ/Reminder">儲存庫</a> · <a href="https://github.com/pretextQ/Reminder/blob/master/docs/architecture.md">架構與邊界</a></p>
</td></tr>
</table>

## 近期進展

- **2026-11-01 · MewCode** — 已交付通知 Outbox、策略恢復重新驗證與重複 pytest 失敗回饋；獨立審查與監督預設關閉，模型效果與生產收益仍待驗證。[交付紀錄](https://github.com/pretextQ/MewCode/blob/master/docs/README.md)

- **2026-10-22 · eyes** — 完成安全與資源邊界修復，靜態與建置檢查通過，新增執行驗收仍待複驗；體驗改進保留於待辦中。[維護紀錄](https://github.com/pretextQ/eyes/blob/main/docs/Review/2026-10-22-remediation.md) · [設計待辦](https://github.com/pretextQ/eyes/blob/main/docs/TODO.md)

- **Reminder · 按需維護** — 已有混合分數融合、可選 BGE-M3 sparse 檢索與飛書 / Confluence 唯讀 MCP 接入。[飛書](https://github.com/pretextQ/Reminder/blob/master/docs/feishu-wiki-mcp.md) · [Confluence](https://github.com/pretextQ/Reminder/blob/master/docs/confluence-mcp.md)

## 技術方向

Agent Runtime · Evaluation & Observability · Memory & Retrieval · Infrastructure

關注權限邊界、有界執行和可追溯證據。分別記錄實作、真實驗證與待辦。

---

<sub>狀態核對：2026-11-01（Asia/Shanghai） · eyes c94d500 / MewCode cc98162 / Reminder 9f1d5e8</sub>
