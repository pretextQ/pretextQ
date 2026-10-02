<p align="center">
  <img src="./assets/profile-hero.svg" alt="pretextQ — Framework, Knowledge, Verification" width="100%" />
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
  AI Agent Framework · Enterprise RBAC · RAG · 檢索評估 · 介面自動化測試 · DevOps 工程化
</p>

<p align="center">
  <a href="https://github.com/pretextQ">GitHub</a>
</p>

---

## 我建構的，不是三個互不相關的儲存庫

我習慣把「能不能落進企業生產環境」當作系統的第一指標：模型只是其中的判斷元件，真正決定成敗的，是權限邊界、資料歸屬、檢索品質、驗證手段和可觀測性。

這三個專案恰好覆蓋了我最關心的完整工程鏈路：

<table>
  <tr>
    <td width="33%" valign="top">
      <strong>⚙️ Agent Framework</strong><br/><br/>
      Agent 如何以登入使用者的身分、在企業權限體系內呼叫內部系統：統一入口、權限繼承、可插拔整合、Trace 與稽核。
    </td>
    <td width="33%" valign="top">
      <strong>🧠 Knowledge / RAG</strong><br/><br/>
      文件如何變成可查核的知識：多模態解析、混合檢索、引用查核，以及可以量化的效果評估。
    </td>
    <td width="33%" valign="top">
      <strong>🛡️ Verification / Testing</strong><br/><br/>
      如何證明系統行為正確：YAML 資料驅動、介面與資料庫雙重校驗、持續迴歸與 CI 閉環。
    </td>
  </tr>
</table>

```text
atlas-claw              →  企業 Agent 的統一入口、權限繼承與可插拔整合
GraphScholarV1          →  把文件變成可查核的知識：混合檢索與引用閉環
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

### 我在這個專案中關注的問題

Agent 在企業裡最常見的失敗不是「不夠聰明」，而是權限失控、整合散落、行為無法追溯。atlas-claw 把這三件事放在設計起點：讓模型的每一次行動都在既有權限體系內、有 Trace 可查、可被報表統計。

**Core Stack**

`Python` `FastAPI` `SQLAlchemy` `JWT / RBAC` `SQLite / MySQL / PostgreSQL` `Docker Compose` `GitHub Actions`

---

## 02 / GraphScholarV1

### [A multimodal RAG knowledge base with verifiable answers](https://github.com/pretextQ/GraphScholarV1)

> 把文件沉澱為可檢索、可引用的知識，讓每一次回答都能回到來源。

| 專案形態 | 核心定位 | 評估體系 | Links |
|---|---|---|---|
| Full-stack RAG 應用 | 多模態知識庫問答 | 內建 RAGAS 評估 | [Repository](https://github.com/pretextQ/GraphScholarV1) |

GraphScholarV1 是面向個人知識管理場景的多模態 RAG 知識庫問答系統：上傳文件 → 智慧解析 → 混合索引 → 精準問答。前後端分離，FastAPI + SQLAlchemy 承載服務層，React 18 + Vite + Tailwind 建構工作台。

### 一條把答案釘在來源上的檢索管道

```text
上傳文件（PDF / Markdown / 掃描件 / 圖片）
      ↓
解析 · 切分 · OCR / VLM 圖片描述
      ↓
BM25 稀疏召回  +  ChromaDB 稠密召回
      ↓
查詢改寫 → 問題路由 → Rerank 精排 → 引用查核
      ↓
回答 + 引用來源（低信心度自動二次檢索）
```

- **多模態文件解析**：PDF / Markdown / 純文字，掃描版走 OCR，圖片交給 VLM 生成描述並參與檢索。
- **混合檢索**：BM25 稀疏召回 + 向量稠密召回雙路並行，兼顧關鍵字命中與語義相似。
- **完整檢索管道**：查詢改寫、問題路由、Rerank 精排、引用查核，低信心度自動觸發二次檢索。
- **增量索引**：檔案 Hash 指紋識別改動，自動重建索引，服務不重啟、知識不中斷。
- **分層快取**：QA 精確匹配 → 向量語義快取 → 正常檢索，命中即省一次完整管道開銷。
- **效果可度量**：內建 RAGAS 評估模組，檢索與生成品質有數字可看，而不是憑感覺調 Prompt。

### 我在這個專案中關注的問題

RAG 系統最難的不是把向量存進去，而是回答是否忠於文件：每句話能不能點回原文引用、檢索失敗時系統如何自證、效果變化如何被測量。GraphScholarV1 把引用查核與 RAGAS 評估當作一級公民。

**Core Stack**

`Python 3.14` `FastAPI` `React 18` `PostgreSQL` `ChromaDB` `BM25` `Reranker` `RAGAS` `LangChain`

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

### 我在這個專案中關注的問題

自動化測試的價值不在跑得多，而在失敗時能否直接定位、成功時能否信任。回應與資料庫的雙重校驗、清晰的分層結構，讓每一次斷言都可解釋、可維護。

**Core Stack**

`Python` `Pytest` `YAML` `JSONPath` `SQL` `Allure` `Docker` `GitHub Actions` `飛書`

---

## 三個專案背後的統一方法

| 原則 | 我的工程取向 |
|---|---|
| **權限先行** | Agent 以登入使用者身分行動，RBAC 決定能看見什麼、呼叫什麼，未授權的整合不進平台層。 |
| **Evidence before answers** | 回答要有引用、斷言要有資料、發現要有證據，不輸出無法回溯的結論。 |
| **薄核心，可替換** | 框架、檢索、測試都以薄核心組織，具體實作做成可插拔元件。 |
| **可觀測與可稽核** | Trace、稽核報表、Allure 報告先於功能堆疊，行為可重放、可統計。 |
| **一切進 CI** | Stub 迴歸、RAGAS 評估、容器化執行都進 GitHub Actions，文件與程式碼同步演進。 |

## 技術版圖

| Layer | Technologies | What I build |
|---|---|---|
| **Agent & Backend** | Python, FastAPI, SQLAlchemy, JWT / RBAC | 服務層、領域模型、權限繼承與對話管理 |
| **RAG & Data** | BM25, ChromaDB, Reranker, PostgreSQL, MySQL | 混合檢索、引用查核、快取與索引治理 |
| **Testing & Quality** | Pytest, YAML, JSONPath, SQL, RAGAS | 介面迴歸、雙重校驗、檢索效果評估 |
| **Delivery & Ops** | Docker, Docker Compose, GitHub Actions, Allure, 飛書 | CI、環境編排、報告與結果通知 |

## 現在仍在推進

- 打磨 atlas-claw 的內嵌 / 獨立雙模式，擴充內部系統的 Provider 與稽核報表
- 以 RAGAS 評估驅動 GraphScholarV1 的檢索品質最佳化與多模態解析擴充
- 沉澱 api-auto-test-framework 的校驗器與用例庫
- Lyra4DAgent · AgentKit · MewCode 等實驗儲存庫的持續迭代

---

<h3 align="center">Build the agent. Ground it in facts. Prove it works.</h3>

<p align="center">
  如果你也在做企業 Agent 落地、RAG 檢索品質或測試工程化，歡迎交流。
</p>

<p align="center">
  <a href="https://github.com/pretextQ">Explore my repositories</a>
</p>
