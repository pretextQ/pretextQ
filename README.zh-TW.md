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
  Coding Agent 與告警自動化 · Agent 評估與執行觀測 · 介面自動化測試 · DevOps 工程化
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## 三個儲存庫，從自動修復到 Agent 評估與介面驗證

我習慣把「能不能落進企業生產環境」當作系統的第一指標：模型只是其中的判斷元件，真正決定成敗的，是權限邊界、工具與上下文治理、驗證手段和可觀測性。

這三個專案涵蓋自動修復、Agent 評估與介面驗證：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 如何無人值守地把線上告警變成可人審的修復 PR：OS 級沙箱、唯讀工具鏈、結構化證據鏈與有界重試。
    </td>
    <td width="33%" valign="top">
      <strong>👁️ Agent Evaluation / Observation</strong><br/><br/>
      用外部測試集與自訂評分驗證 Agent，關聯執行軌跡、評分證據與版本迴歸報告。
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      如何證明系統行為正確：YAML 資料驅動、介面與資料庫雙重校驗、持續迴歸與 CI 閉環。
    </td>
  </tr>
</table>

```text
MewCode                 →  告警進來、PR 出去：沙箱裡的無人值守修復
eyes                    →  Agent 測試、執行觀測與有證據的迴歸比較
api-auto-test-framework →  用資料驅動與雙重校驗證明每一次介面行為
```

---

## 01 / MewCode

### [An AI Coding Agent that turns alerts into reviewed PRs](https://github.com/pretextQ/MewCode)

> 同一個 agent 核心，兩種形態：互動式 CLI/TUI 幫你寫程式碼；無頭服務常駐接線上告警——agent 在 OS 級沙箱裡定位、修復、驗證，以 PR 交給人工 review。人審 PR 是唯一必經的人工節點，服務沒有任何 merge 權限。

| 專案形態 | 核心定位 | 信任邊界 | Links |
|---|---|---|---|
| AI Coding Agent + 告警驅動的自動化開發服務 | 告警 → 修復 PR · OS 級沙箱 · 有界重試 | 人審 PR 唯一關卡 · 服務無 merge 權限 | [Repository](https://github.com/pretextQ/MewCode) |

MewCode 把「收到告警 → 定位 → 修復 → 驗證 → 提交 review」這條值班鏈路自動化：Alertmanager webhook 觸發，每個 job 在獨立沙箱與 git worktree 中執行，PR body 自帶結構化證據鏈——告警摘要、根因、測試前後對比、整合驗證——審查者不看 agent 日誌也能做判斷。真機評估集回放 6 單全部無人值守跑通：修復 PR 成功率 100%，告警→PR 中位數 25–26 秒。

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

- **告警進來，PR 出去**：demo 儲存庫保留歷次無人值守執行的真實 PR，每份都是完整證據鏈，可從 [PR #5](https://github.com/pretextQ/mewcode-alert-demo/pull/5) 看起。
- **修不好就誠實升級**：重試有上限，超限進入 escalate 並附上已嘗試的分析；驗證不過的修復不會被發布，絕不產生垃圾 PR。
- **OS 級沙箱**：agent 在容器內執行——非 root、全部能力丟棄、唯讀根檔案系統、CPU/記憶體/PID 限制、硬逾時強制終止，宿主設定與金鑰不進容器。
- **內部工具鏈唯讀**：內建生產日誌（Loki）與 CI 狀態（GitHub）兩個 MCP server，讀取路徑只有 GET；未聲明 readOnlyHint 的工具在服務模式一律不掛。
- **自起測試環境**：儲存庫帶 docker-compose.yml 時自動啟動依賴，在沙箱內跑整合測試；無容器執行環境則如實記錄「未執行」，不用假驗證充數。
- **營運可量化**：/metrics（Prometheus）、單 job JSON 複盤、按儲存庫 token 成本；評估集回放讓每次提示詞/核心改動都有成功率、MTTR 與成本的前後對比。
- **多儲存庫策略**：`.mewcode/policy.yaml` 聲明觸發路由、目標分支、token 預算與通知管道，按 job 即時讀取、改檔案即生效，損壞的策略快速失敗而非靜默回退。
- **互動式核心同樣完整**：工具集 + 權限分層管線（deny 優先於白名單）+ Skill 與 MCP 擴充，Windows 是一等公民平台。

### 這個專案真正要解決的問題

無人值守寫程式碼最難的不是「修得快」，而是「憑什麼信」：權限怎麼收斂、行為怎麼隔離、證據怎麼沉澱、改壞了怎麼保證不上線。MewCode 把信任當成設計起點——headless 模式下「要問人」的動作一律拒絕而非放行，PR 是唯一出口，人是唯一關卡。

**Core Stack**

`Python 3.11` `Textual` `asyncio` `Docker 沙箱` `MCP` `Prometheus` `SQLite` `GitHub Actions`

---

## 02 / eyes

### [Agent evaluation and execution observation](https://github.com/pretextQ/eyes)

> 用外部測試集、自訂評分與執行證據，驗證 Agent 修改是否有效，並看清每次任務如何執行。

| 專案形態 | 核心定位 | 目前狀態 | Links |
|---|---|---|---|
| 自託管 Agent 測試與執行觀測平台 | 並行實驗 · 自訂評分 · 執行證據 · 迴歸比較 | 控制後端、Runner、SDK 與 Web 控制台已實作；部分驗收待完成 | [Repository](https://github.com/pretextQ/eyes) |

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
- **驗證邊界透明**：已完成 Deta、Zeta 兩個真實 Python Agent 的並行與取消驗證；真實 HTTP Agent、Runner 整體重啟恢復及容量驗收仍待完成。

**Core Stack**

`Python 3.14` `FastAPI` `SQLAlchemy` `PostgreSQL` `OpenTelemetry` `React` `TypeScript` `Vite`

---

## 03 / api-auto-test-framework

### [YAML-driven API testing with dual validation](https://github.com/pretextQ/api-auto-test-framework)

> 用 YAML 資料驅動 + 雙重校驗，把介面測試做成可以持續迴歸的工程資產，而不是一次性腳本。

| 專案形態 | 資料驅動 | 校驗方式 | Links |
|---|---|---|---|
| 介面自動化測試框架 | YAML 用例 | JSONPath + SQL | [Repository](https://github.com/pretextQ/api-auto-test-framework) |

基於 Pytest 的微服務介面自動化測試框架：克隆、裝依賴、`pytest` 三步跑通，不需要額外設定腳本。單 YAML 檔案即可覆蓋單介面冒煙、異常斷言與多步依賴鏈路。

### 把介面測試做成可迴歸的工程資產

```text
YAML 用例（單介面 / 異常 / 鏈路）
      ↓
Pytest 執行引擎 + 上下文傳參（extract → 模板）
      ↓
JSONPath 回應斷言  +  SQL 資料校驗
      ↓
Allure 報告 · 飛書結果通知
      ↓
Docker 化執行 · GitHub Actions CI
```

- **YAML 資料驅動**：用例即文件，單檔案覆蓋冒煙、異常與多步鏈路場景。
- **鏈路上下文傳參**：extract 提取 + 參數模板，跨介面取數像讀句子一樣自然。
- **JSONPath + SQL 雙重校驗**：介面回應與資料庫狀態同時斷言，防住「回傳 200 但資料沒落庫」這類問題。
- **多環境切換**：conftest 管理環境組態，一套用例跑遍開發、測試、預發。
- **結果工程閉環**：Allure 報告、飛書機器人通知、Docker 化執行、GitHub Actions CI 全部就位。

### 這個專案真正要解決的問題

自動化測試的價值不在跑得多，而在失敗時能否直接定位、成功時能否信任。回應與資料庫的雙重校驗、清晰的分層結構，讓每一次斷言都可解釋、可維護。

**Core Stack**

`Python` `Pytest` `YAML` `JSONPath` `SQL` `Allure` `Docker` `GitHub Actions` `飛書`

---

## 貫穿三個專案的工程原則

| 原則 | 我的工程取向 |
|---|---|
| **權限先行** | Agent 工具遵循權限分層與唯讀邊界，修復在沙箱內執行，PR 經人工審查。 |
| **Evidence before answers** | 回答要有依據、斷言要有資料、發現要有證據，不輸出無法回溯的結論。 |
| **薄核心，可替換** | Agent 核心與測試都以薄核心組織，具體實作做成可插拔元件。 |
| **可觀測與可稽核** | 執行指標、任務複盤、Allure 報告先於功能堆疊，行為可重放、可統計。 |
| **一切進 CI** | 評估集重放、pytest + ruff + mypy 檢查、容器化執行都進 GitHub Actions，文件與程式碼同步演進。 |

## 技術分層

| Layer | Technologies | What I build |
|---|---|---|
| **Agent Internals** | Textual, asyncio, MCP, Docker, Prometheus | Agent 核心、權限分層、沙箱執行與告警服務化 |
| **Agent Evaluation & Web** | Python, FastAPI, SQLAlchemy, PostgreSQL, OpenTelemetry, React, TypeScript | 服務控制面、獨立 Runner、執行軌跡、評分與迴歸報告 |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, ruff, mypy | 介面迴歸、雙重校驗、靜態型別與風格檢查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, 飛書 | CI、環境編排、報告與結果通知 |

## 最近在做的事

- 打磨 MewCode 的沙箱 egress 白名單與告警觸達，擴充評估集用例
- 沉澱 api-auto-test-framework 的校驗器與用例庫

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企業 Agent 落地、Coding Agent 原理實作或測試工程化，歡迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
