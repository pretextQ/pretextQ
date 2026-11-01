# pretextQ

建構可驗證、可觀測、權限可控的 Agent 系統。

<a href="./README.md">简体中文</a> · <strong>繁體中文</strong> · <a href="./README.en.md">English</a>

## 精選專案

<table width="100%">
<tr><td width="9999">
<h3><img src="./assets/eyes.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/eyes">eyes</a></h3>
<p><strong>Agent 評測與執行觀測</strong><br/>
用自訂測試集、評分規則和執行證據，判斷 Agent 修改是否有效。</p>
<p>版本化實驗 · 獨立評分 · 執行軌跡 · 迴歸比較<br/>
<sub>Python / FastAPI / React / PostgreSQL</sub></p>
<p><a href="https://github.com/pretextQ/eyes">儲存庫</a> · <a href="https://github.com/pretextQ/eyes#两条使用路径">接入與使用</a><br/>
<sub>v0.1.0 · 單團隊自託管，按需使用與維護</sub></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><img src="./assets/mewcode.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/MewCode">MewCode</a></h3>
<p><strong>從線上告警到可審查的修復 PR</strong><br/>
在獨立 worktree 與 Docker 沙箱中定位、修復和驗證，再交給人工審查。</p>
<p>有界重試 · 唯讀 MCP · 通知 Outbox · 獨立審查<br/>
<sub>Python / Textual / Docker / SQLite</sub></p>
<p><a href="https://github.com/pretextQ/MewCode">儲存庫</a> · <a href="https://github.com/pretextQ/mewcode-alert-demo/pull/5">真實修復 PR</a><br/>
<sub>v0.3.0 · CLI / TUI 與告警服務共用 Agent 核心；服務無合併權限</sub></p>
</td></tr>
</table>

<table width="100%">
<tr><td width="9999">
<h3><img src="./assets/reminder.svg" width="24" height="24" alt="" /> <a href="https://github.com/pretextQ/Reminder">Reminder</a></h3>
<p><strong>有來源、可治理的長期記憶</strong><br/>
將筆記、文件與經歷沉澱為可檢索、可追溯的個人記憶，為 Mneme 知識工作台提供 Agent 能力。</p>
<p>混合檢索 · 記憶治理 · 引用驗證 · 可恢復執行<br/>
<sub>Python / FastAPI / Vue / pgvector / BGE-M3</sub></p>
<p><a href="https://github.com/pretextQ/Reminder">儲存庫</a> · <a href="https://github.com/pretextQ/Reminder/blob/master/docs/architecture.md">架構與邊界</a><br/>
<sub>v0.1.0 · 線上回答統一經過 Reminder API</sub></p>
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
