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
  AI Agent Framework · Enterprise RBAC · Coding Agent 原理與實作 · 介面自動化測試 · DevOps 工程化
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## 三個儲存庫，一條完整的工程鏈路

我習慣把「能不能落進企業生產環境」當作系統的第一指標：模型只是其中的判斷元件，真正決定成敗的，是權限邊界、工具與上下文治理、驗證手段和可觀測性。

這三個專案恰好覆蓋了我最關心的完整工程鏈路：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>⚙️ Agent Framework</strong><br/><br/>
      Agent 如何以登入使用者的身分、在企業權限體系內呼叫內部系統：統一入口、權限繼承、可插拔整合、Trace 與稽核。
    </td>
    <td width="33%" valign="top">
      <strong>🧩 Coding Agent</strong><br/><br/>
      AI Coding Agent 的核心原理如何落地為工程實作：Agent 主迴圈、工具呼叫、權限分層、MCP 擴充與上下文管理。
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      如何證明系統行為正確：YAML 資料驅動、介面與資料庫雙重校驗、持續迴歸與 CI 閉環。
    </td>
  </tr>
</table>

```text
atlas-claw              →  企業 Agent 的統一入口、權限繼承與可插拔整合
MewCode                 →  完整實作 AI Coding Agent：主迴圈、工具與權限
api-auto-test-framework →  用資料驅動與雙重校驗證明每一次介面行為
```

---

## 01 / atlas-claw

### [An enterprise Agent framework with a thin core](https://github.com/pretextQ/atlas-claw)

> 讓 Agent 以登入使用者的身分、在企業權限體系內呼叫內部系統完成任務——而不是在平台層散落硬編碼整合。

| 專案形態 | 核心定位 | 目前狀態 | Links |
|---|---|---|---|
| 企業級 AI Agent 框架 | 統一入口 · RBAC 繼承 · 可插拔 Provider | v0.1.0-alpha · 活躍開發 | [Repository](https://github.com/pretextQ/atlas-claw) |

atlas-claw 是面向企業場景的 AI Agent 框架：用一個統一對話入口打通 CRM、ITSM、監控、HR、財務、OA 等內部系統。平台層保持薄核心，只負責路由、鑑權與編排；具體系統接入全部做成可插拔 Provider。

### 讓 Agent 真正落進企業內部網路

```text
使用者（企業帳號 / JWT）
      ↓
FastAPI 服務層 ── 對話 · 稽核 · Trace 落庫
      ↓
Agent 編排（薄核心）
      ↓
可插拔 Provider
      ↓
CRM · ITSM · 監控 · HR · 財務 · OA
```

- **統一對話入口**：一個入口串聯多個內部系統，使用者不需要知道背後接了哪個 Provider。
- **嚴格繼承 RBAC 權限**：對話身分即系統身分，唯讀使用者只能看見自己有權限的系統與資料。
- **薄核心 + 可插拔 Provider**：新系統接入寫成獨立 Provider，內部直呼叫既有介面，不在平台層堆積硬編碼整合。
- **內嵌 / 獨立兩種部署**：既可嵌進企業現有系統共享使用者與組織架構，也可獨立部署在內部網路自託管。
- **可觀測與可稽核**：每次 Provider 呼叫的參數、耗時、狀態碼落庫成 Trace，附稽核報表、失敗率與耗時分析。
- **為測試而設計**：核心不綁定具體模型，CI 中以固定 Stub Provider 跑端到端迴歸。

### 這個專案真正要解決的問題

Agent 在企業裡最常見的失敗不是「不夠聰明」，而是權限失控、整合散落、行為無法追溯。atlas-claw 把這三件事放在設計起點：讓模型的每一次行動都在既有權限體系內、有 Trace 可查、可被報表統計。

**Core Stack**

`Python` `FastAPI` `SQLAlchemy` `JWT / RBAC` `SQLite / MySQL / PostgreSQL` `Docker Compose` `GitHub Actions`

---

## 02 / MewCode

### [A complete AI Coding Agent implementation, from first principles](https://github.com/pretextQ/MewCode)

> 把 AI Coding Agent 的完整實作攤開來看：Agent 主迴圈、工具呼叫、權限分層、MCP 擴充——可讀、可跑、可改。

| 專案形態 | 核心定位 | 介面形態 | Links |
|---|---|---|---|
| AI Coding Agent 完整實作 | Agent 原理 · 工具系統 · 權限分層 | Textual 終端 TUI | [Repository](https://github.com/pretextQ/MewCode) |

MewCode 是一個 AI Coding Agent 的完整實作，目標是把 AI Agent 的核心原理與工程實務講清楚：不只是「會呼叫模型的迴圈」，而是把工具系統、權限模型、上下文與記憶管理、MCP 擴充一一實作到位。介面以 Textual 建構，Windows 是一等公民執行平台。

### 一個 Coding Agent 的完整骨架

```text
使用者輸入（Textual TUI）
      ↓
Agent 主迴圈（asyncio 驅動）
      ↓
LLM 用戶端（anthropic / openai / openai-compat 三種協定）
      ↓
工具呼叫 ── 內建工具 · Skills · MCP（stdio / Streamable HTTP）
      ↓
權限分層 ── 敏感操作彈窗確認，不靜默執行
      ↓
上下文與記憶管理 · Hooks · 檔案歷史
      ↓
多 Agent 協作（teams / worktree）
```

- **完整 Agent 主迴圈**：asyncio 驅動的對話迴圈，內建 Plan / AskUser / Permission 對話框，Agent 每一步行為可見、可控。
- **多協定模型接入**：anthropic / openai / openai-compat 三種協定即配即用，支援 extended thinking，API Key 走環境變數回退。
- **可擴充的工具系統**：內建檔案與指令工具，Skills 與 Memory 沉澱能力，MCP 以 stdio / Streamable HTTP 雙傳輸接入外部工具。
- **權限分層語義**：敏感操作彈出確認對話框而不是靜默執行，權限語義寫成文件，危險動作有明確邊界。
- **Hooks 契約**：stdin JSON 契約清晰，生命週期鉤子可觀測、可攔截，擴充點行為可預期。
- **多 Agent 協作**：teams 與 worktree 做任務隔離，teammate 樹狀管理，檔案歷史可回溯。
- **Windows 一等公民**：GBK 編碼讀取、行尾保持、行程樹清理等平台邊界有專門文件與處理。

### 這個專案真正要解決的問題

AI Coding Agent 的內部實作大多封裝在商業產品裡：會用的人多，能說清主迴圈怎麼轉、工具怎麼編排、權限怎麼攔截的人少。MewCode 把這層黑盒打開——每一步都是可讀、可跑、可改的工程程式碼，而不是概念圖。

**Core Stack**

`Python 3.11` `Textual` `asyncio` `MCP` `uv` `pytest` `ruff` `mypy` `GitHub Actions`

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
| **權限先行** | Agent 以登入使用者身分行動，RBAC 決定能看見什麼、呼叫什麼，未授權的整合不進平台層。 |
| **Evidence before answers** | 回答要有依據、斷言要有資料、發現要有證據，不輸出無法回溯的結論。 |
| **薄核心，可替換** | 框架、Agent 內核、測試都以薄核心組織，具體實作做成可插拔元件。 |
| **可觀測與可稽核** | Trace、稽核報表、Allure 報告先於功能堆疊，行為可重放、可統計。 |
| **一切進 CI** | Stub 迴歸、pytest + ruff + mypy 檢查、容器化執行都進 GitHub Actions，文件與程式碼同步演進。 |

## 技術分層

| Layer | Technologies | What I build |
|---|---|---|
| **Agent & Backend** | Python, FastAPI, SQLAlchemy, JWT / RBAC | 服務層、領域模型、權限繼承與對話管理 |
| **Agent Internals** | Textual, asyncio, MCP, Hooks, uv | Agent 主迴圈、工具系統、權限分層與上下文管理 |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, ruff, mypy | 介面迴歸、雙重校驗、靜態型別與風格檢查 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, 飛書 | CI、環境編排、報告與結果通知 |

## 最近在做的事

- 打磨 atlas-claw 的內嵌 / 獨立雙模式，擴充內部系統的 Provider 與稽核報表
- 迭代 MewCode 的多 Agent 協作與 Skills 生態，補齊 Hooks 與 MCP 的邊界場景
- 沉澱 api-auto-test-framework 的校驗器與用例庫

---

<h3 align="center">Agents you can actually ship.</h3>

<p align="center">
  如果你也在做企業 Agent 落地、Coding Agent 原理實作或測試工程化，歡迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
