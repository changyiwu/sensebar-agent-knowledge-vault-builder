# sensebar-agent-knowledge-vault-builder（專案藍圖）

> 本檔為跨 Agent 通用的專案藍圖（AGENTS.md 開放標準）。任何 Agent 的每個 session 都應先讀本檔＋`handoff.md`。

## 專案簡介

透過「動手實作」與「循序漸進」的方式，從零開始理解並學習 AI Agent 的核心概念與設計模式。同時包含 Sensebar 影片字幕擷取工具，把外部影音內容整理成可用的知識素材。

開發環境：Python 3.10+，核心依賴 `google-generativeai`（Gemini-2.5-flash / 1.5-flash 作為大腦）、`python-dotenv`、`colorama`。

## 關鍵時程

<!-- 目前無固定時程 -->

## 目標與路線圖

### Stage 1：基礎 ReAct（Reasoning & Action）Agent — 已完成

從零手寫 ReAct 循環，理解 Agent 如何自主思考、選擇工具、觀察結果。

- [x] 設計 System Prompt 讓大模型理解 ReAct 規則（Thought → Action → Observation）
- [x] 提供簡單的自訂 Python 函數作為 Tool（如計算機、模擬搜尋）
- [x] 實作 Agent Loop 控制器，解析模型輸出並調用 Tool
- [x] 使用彩色終端輸出，視覺化展示思考過程

### Stage 2：工具與函數調用（Function Calling）— 當前目標

從正則表達式解析／純文字 Prompt 轉換成原生的 Function Calling 機制。

- [ ] 學習如何將 Python 函數轉換成 API 規範的 JSON Schema
- [ ] 使用 Gemini / OpenAI 的 Native Function Calling
- [ ] 提升工具調用的穩定性與結構化參數解析

### Stage 3：記憶與狀態管理（Memory & State）

- [ ] 實作滑動視窗（Sliding Window）或摘要型記憶以避免 Token 爆炸
- [ ] 儲存對話歷史到本地 JSON / SQLite
- [ ] 建立簡單的用戶 Profile 記憶

### Stage 4：檢索增強生成（RAG）整合

- [ ] 文件切片（Chunking）與向量化（Embedding）
- [ ] 串接 Vector Database（如 ChromaDB、FAISS）
- [ ] 實作「檢索工具」供 Agent 自主調用

### Stage 5：多 Agent 協作（Multi-Agent System）

- [ ] 角色定義（Role Playing）與任務分配
- [ ] 實作路由機制（Routing）或群聊機制（Group Chat）
- [ ] 引入人類反饋機制（Human-in-the-loop）

### 素材工具線

- [x] `download_all_subs.py`、`extract_videos.py` 改為可攜路徑（原始固定目錄不存在時改用專案目錄）
- [x] 影片擷取同步更新 Markdown 與 URL 清單，加入 EP01 影片
- [ ] 在目前專案目錄實際執行擷取流程，確認字幕與清單輸出
- [ ] 補齊 `requirements.txt` 與可重複執行的驗證流程

## 資料夾結構

```
sensebar-agent-knowledge-vault-builder/
├─ download_all_subs.py       # 批次下載字幕
├─ extract_videos.py          # 擷取影片清單，同步 Markdown 與 URL 清單
├─ sensebar_ai_videos.md      # 影片清單（Markdown）
├─ sensebar_ai_urls.txt       # 影片網址清單
├─ subtitles/                 # 下載的字幕
├─ README.md                  # 專案導覽與概念說明
├─ agents.md                  # 本檔：專案藍圖
├─ handoff.md                 # 交接檔（每次收工必更新）
└─ .gitignore
```

> 尚未建立：`requirements.txt`（依賴定義）、`tools.py`（自訂工具庫）、`simple_agent.py`（ReAct 核心邏輯）。

## 同步層級（本專案初始化至第 3 層級）

| 層級 | 平台 | 位置 | 讀取時機 |
|------|------|------|---------|
| L1 | 本地（GDrive） | `agents.md`＋`handoff.md` | 每個 session |
| L2 | GitHub | https://github.com/changyiwu/sensebar-agent-knowledge-vault-builder （公開） | 指定時 |
| L3 | Obsidian | `sensebar-agent-knowledge-vault-builder/專案工作流程.md` | 有需要時 |

## 工作約定

- 任何 Agent、任何電腦：**開工先讀 `handoff.md`，收工必更新 `handoff.md`**
- 修改共用檔案前先讀最新內容，避免覆蓋其他 Agent 的變更
- 所有回應與文件使用繁體中文
- 修改前先檢查 Git 狀態，只處理本次任務相關變更
- 不提交 API key、登入 cookie、下載憑證或私人字幕來源

## 字幕清理規則

`download_all_subs.py` 以 `yt-dlp` 下載 VTT 字幕後，必須依序執行下列清洗步驟：

1. 移除 VTT 元資料檔頭與時間戳記（如 `00:00:01.000 --> 00:00:03.000`）
2. 移除 HTML／XML 標籤
3. **滾動式去重**：YouTube 自動字幕會有重複的滾動行，需以連續行去重演算法生成流暢段落
4. 輸出為 Markdown，將影片標題作為 H1，並包含原始 YouTube 連結

## 知識維護規範

- `01-Clippings/` 下的原始字幕檔**不可直接修改**（外部輸入）
- 知識庫整理成果輸出至 `02-知識庫/`，並主動更新其 `index.md` 與 `log.md`
- Vault 實際結構為編號四層：`00-每日筆記/`、`01-Clippings/`、`02-知識庫/`、`03-創作庫/`

## 最近進度

- 2026-07-22：改善影片清單工具的可攜路徑與網址清單輸出，新增 EP01 影片資料，並登記 Obsidian L3 專案筆記位置；Python 語法驗證通過。
- 2026-07-24：專案藍圖改用標準範本格式（Roadmap 的 mermaid 圖改為 checklist 分階段，補上資料夾結構與同步層級表）。
