<p align="center">
  <img src="./assets/profile-hero.svg" alt="pretextQ — Agents you can actually ship" width="100%" />
</p>

<p align="center">
  <a href="./README.md">简体中文</a>
  ·
  <strong>繁體中文</strong>
  ·
  <a href="./README.en.md">English</a>
</p>

<h1 align="center">你好，我是 pretextQ 👋</h1>

<h3 align="center">我建構的不是「會跑的 Demo」，而是權限可控、結果可驗證、能落進企業生產環境的系統。</h3>

<p align="center">
  Agent 評估與執行觀測 · Coding Agent 與告警自動化 · 長期記憶與知識工作台 · DevOps 工程化
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## 三個儲存庫，涵蓋 Agent 評估、執行與長期記憶

我習慣把「能不能落進企業生產環境」當作系統的第一指標：模型只是其中的判斷元件，真正決定成敗的，是權限邊界、工具與上下文治理、驗證手段和可觀測性。

這三個專案分別聚焦 Agent 執行評估、自動修復與長期記憶：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>👁️ Agent Evaluation / Observation</strong><br/><br/>
      用外部測試集與自訂評分驗證 Agent，關聯執行軌跡、評分證據與版本迴歸報告。
    </td>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 如何無人值守地把線上告警變成可人審的修復 PR：OS 級沙箱、唯讀工具鏈、結構化證據鏈與有界重試。
    </td>
    <td width="33%" valign="top">
      <strong>🧠 Memory / Knowledge</strong><br/><br/>
      將筆記、文件與經歷沉澱為長期記憶，透過混合檢索、記憶治理與引用驗證保留回答依據。
    </td>
  </tr>
</table>

```text
eyes                    →  Agent 測試、執行觀測與有證據的迴歸比較
MewCode                 →  告警進來、PR 出去：沙箱裡的無人值守修復
Reminder                →  長期記憶、混合檢索與可追溯的知識工作台
```

---

## 01 / eyes

### [Agent evaluation and execution observation](https://github.com/pretextQ/eyes)

> 用外部測試集、自訂評分與執行證據，驗證 Agent 修改是否有效，並看清每次任務如何執行。

| 專案形態 | 核心定位 | 目前狀態 | Links |
|---|---|---|---|
| 自託管 Agent 測試與執行觀測平台 | 並行實驗 · 自訂評分 · 執行證據 · 迴歸比較 | v0.1.0 · 目前約定範圍已交付，按需使用與維護 | [Repository](https://github.com/pretextQ/eyes) |

eyes 將目標 Agent、測試集版本、評分器版本和執行設定綁定到實驗，關聯用例執行、評分紀錄、軌跡與產物。支援 HTTP/Python 接入、獨立評分程序、多 Agent 批次和持久化迴歸報告，也可透過 SDK 直接觀測本地 Agent 對話，無需先建立實驗。

### 從執行過程到迴歸證據

```text
目標 Agent + JSONL 測試集 + 自訂評分器
      ↓
控制後端與排程器 ── 固定版本 · 並行限制
      ↓
獨立 Runner ── 執行嘗試 · 軌跡 · 產物
      ↓
獨立評分程序 ── 判定 · 理由 · 證據引用
      ↓
Web / CLI / API ── 迴歸報告 · CI 品質門檻
```

- **評分可追溯**：結論關聯用例、執行嘗試、評分口徑和實際證據，區分執行失敗、評分失敗與證據不足。
- **執行過程可觀測**：展示已採集的模型呼叫、工具執行和呼叫關係，明確採集覆蓋範圍與缺失狀態。
- **迴歸口徑明確**：固定實驗設定，按用例版本比較改善與退化，單獨標記測試集或評分口徑變化帶來的不可比項。
- **執行與評分分離**：獨立 Runner 和評分程序，支援評分取消、重試及證據保留清理。
- **真實接入與維運驗收**：MewCode Python/統一 HTTP、獨立 trae HTTP、雙 Agent 並行與定向恢復已有紀錄；trae 被動觀測、離線補傳、生命週期、容量告警與取消稽核通過各自約定範圍。
- **安全與資源邊界強化**：10 月 22 日完成金鑰引用校驗、遠端明文控制連線明確授權、取消派發競態、事件與詳情上限、證據恢復及本地保留清理等修復。靜態檢查、離線遷移 SQL、wheel 與 Web 建置通過；本輪真實並行、取消、配額與恢復仍待執行複驗，見[維護紀錄](https://github.com/pretextQ/eyes/blob/main/docs/Review/2026-10-22-remediation.md)。
- **驗證邊界透明**：目前面向單團隊自託管、按需執行；完整故障矩陣、長期容量與真實版本改善/退化尚未驗收。編碼專項首輪兩題通過、訂單題失敗，後續重試為 unknown，無有效品質評分，見[真實評測紀錄](https://github.com/pretextQ/eyes/blob/main/docs/coding-regression-real-acceptance-20261016.md)。

**Core Stack**

`Python 3.14` `FastAPI` `SQLAlchemy` `PostgreSQL` `OpenTelemetry` `React` `TypeScript` `Vite`

---

## 02 / MewCode

### [An AI Coding Agent that turns alerts into reviewed PRs](https://github.com/pretextQ/MewCode)

> 互動式 CLI/TUI 與告警驅動服務共用 Agent 核心，在沙箱中完成定位、修復和驗證，以 PR 交給人工審查。

| 專案形態 | 核心定位 | 信任邊界 | Links |
|---|---|---|---|
| AI Coding Agent + 告警驅動的自動化開發服務 | 告警 → 修復 PR · OS 級沙箱 · 有界重試 | 人審 PR 唯一關卡 · 服務無 merge 權限 | [Repository](https://github.com/pretextQ/MewCode) |

### 從告警到 PR 的無人值守鏈路

```text
告警（Alertmanager webhook）
      ↓
服務層 ── SQLite 狀態機 · 指紋去重 · 重啟恢復
      ↓
每 job 隔離單元（git worktree + Docker 沙箱）
      ↓
Agent 核心 ── 定位 · 修復 · 有界重試（超限 escalate）
      ↓
驗證 ── 迴歸測試 + 自起 docker-compose 整合測試
      ↓
PR + CI 關卡 ─▶ 人工 review（唯一上線關卡）
```

- **隔離與權限**：獨立 worktree 與 Docker 沙箱、唯讀 MCP 工具鏈，執行受資源和權限邊界約束。
- **有界修復**：自動串聯定位、修復、迴歸與整合測試；重試超限升級人工，PR 附根因和測試證據。
- **可靠執行**：告警去重、重啟恢復、通知 Outbox 與策略重新驗證，結合指標和 token 用量支援任務複盤。
- **擴充與審查**：支援 Skill/MCP 與 Windows；獨立審查和監督預設關閉，僅提供意見與預算內回饋。
- **驗證與邊界**：已有[真實示範 PR](https://github.com/pretextQ/mewcode-alert-demo/pull/5)和[驗收紀錄](https://github.com/pretextQ/MewCode/blob/master/docs/stage-2026-11-01-dead-end.md)；尚無生產收益結論，細粒度對外連線限制仍需宿主設定。

詳細說明：[服務可靠性](https://github.com/pretextQ/MewCode/blob/master/guides/service-reliability.md) · [獨立審查](https://github.com/pretextQ/MewCode/blob/master/guides/independent-review.md) · [監督指南](https://github.com/pretextQ/MewCode/blob/master/guides/supervision.md)

**Core Stack**

`Python 3.11` `Textual` `asyncio` `Docker 沙箱` `MCP` `Prometheus` `SQLite` `GitHub Actions`

---

## 03 / Reminder

### [The memory Agent at the heart of Mneme](https://github.com/pretextQ/Reminder)

> 將筆記、文件與經歷沉澱為可檢索、可追溯、可持續演化的個人記憶。

| 專案形態 | 核心定位 | 目前狀態 | Links |
|---|---|---|---|
| Mneme 記憶 Agent 與知識工作台 | 混合檢索 · 記憶治理 · 引用驗證 · 可恢復執行 | v0.1.0 · 線上回答統一經過 Reminder API | [Repository](https://github.com/pretextQ/Reminder) |

Reminder 負責檢索、記憶治理、回答生成與引用驗證；Mneme 提供知識庫、文件工作台、知識圖譜、個人畫像和成長分析。兩者擁有獨立資料庫，透過版本化 HTTP 契約通訊，明確資料所有權與服務邊界。

### 讓長期記憶有來源、有邊界

```text
Vue 知識工作台 → Mneme API
      ↓
Reminder API → BGE-M3 + PostgreSQL / pgvector
      ↓
記憶治理 · 回答生成 · 引用驗證
      ↓
耐久 Agent Run · Outbox / Inbox · 可重建投影
```

- **混合檢索與證據引用**：融合語義向量、關鍵字與圖譜資訊，新增正規化後端分數融合與可選 BGE-M3 sparse 文件檢索；稀疏向量未補齊時繼續走 dense + keyword 路徑。
- **知識庫 MCP 接入**：新增指定飛書知識庫與 Confluence Cloud 空間的唯讀目錄、搜尋及原文讀取，見[飛書說明](https://github.com/pretextQ/Reminder/blob/master/docs/feishu-wiki-mcp.md)與[Confluence 說明](https://github.com/pretextQ/Reminder/blob/master/docs/confluence-mcp.md)。
- **記憶治理**：候選、人工治理、修訂歷史和證據關係共同維護長期記憶。
- **可恢復執行**：耐久執行紀錄、租約和冪等語義處理重試及程序中斷。
- **刪除與重建**：刪除 fence 防止舊事件恢復已刪資料，衍生狀態支援安全回填。
- **交付閉環**：Vue 工作台、Compose 服務棧、GHCR 版本映像與 CI 評測一起維護。

**Core Stack**

`Python 3.12` `FastAPI` `Vue 3` `TypeScript` `PostgreSQL / pgvector` `BGE-M3` `Neo4j` `Redis / Celery` `Docker Compose`

---

## 貫穿三個專案的工程原則

| 原則 | 我的工程取向 |
|---|---|
| **權限先行** | Agent 工具遵循權限分層與唯讀邊界，修復在沙箱內執行，PR 經人工審查。 |
| **Evidence before answers** | 回答要有依據、斷言要有資料、發現要有證據，不輸出無法回溯的結論。 |
| **薄核心，可替換** | Agent 核心與測試都以薄核心組織，具體實作做成可插拔元件。 |
| **可觀測與可稽核** | 執行指標、任務複盤、評分證據與引用稽核先於功能堆疊，行為可重放、可統計。 |
| **一切進 CI** | 評估集重放、pytest + ruff + mypy 檢查、容器化執行都進 GitHub Actions，文件與程式碼同步演進。 |

## 技術分層

| Layer | Technologies | What I build |
|---|---|---|
| **Agent Evaluation & Web** | Python, FastAPI, SQLAlchemy, PostgreSQL, OpenTelemetry, React, TypeScript | 服務控制面、獨立 Runner、執行軌跡、評分與迴歸報告 |
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent 核心、權限分層、沙箱執行與告警服務化 |
| **Memory & Knowledge** | Vue, FastAPI, PostgreSQL / pgvector, BGE-M3, Neo4j, Redis / Celery | 長期記憶、知識工作台、圖譜投影與可恢復非同步執行 |
| **Testing & Quality** | Pytest, ruff, mypy, TypeScript | Agent 評測、迴歸驗證、靜態型別與風格檢查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, GHCR, Prometheus | CI、環境編排、版本映像、監控與維運 |

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企業 Agent 落地、Coding Agent 原理實作、Agent 評估或長期記憶系統，歡迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
