# AntiGravity 開發者指南 - sensebar-agent-knowledge-vault-builder

本檔案為 AntiGravity 專案助理的開發指南與規則庫。在對本專案進行任何修改、除錯或新增功能之前，請務必先詳讀此檔案。

---

## 專案概述

*   **專案名稱**：sensebar-agent-knowledge-vault-builder
*   **用途**：自動化從 YouTube 頻道（特別是 @sensebar）下載影片字幕，進行清理與去重，並將整理後的 Markdown 筆記同步至三層式 Obsidian 第二大腦中，供後續教案規劃使用。
*   **專案類型**：Python 腳本工具集，內建 ReAct 循環與工具整合（後續階段會擴充為 Agent 自主維護系統）。
*   **技術棧**：
    *   **核心**：Python 3.10+
    *   **相依套件**：`yt-dlp` (下載字幕/音訊), `google-generativeai` (Gemini API 整合), `python-dotenv` (環境變數), `colorama` (終端彩色輸出)。
    *   **筆記庫**：Obsidian 三層結構 (`Clippings/`, `知識庫/`, `創作庫/`)。

---

## 目錄結構與運作機制

### 1. 目錄結構
*   `extract_videos.py`：從指定頻道獲取影片 URL 並依據關鍵字進行過濾。
*   `download_all_subs.py`：利用 `yt-dlp` 下載 VTT 字幕，並呼叫清洗引擎去除 YouTube 自動產生的滾動重複文字，最後存成乾淨的 Markdown 格式。
*   `agents.md`：專案學習與實作路線圖（本專案之工作藍圖，包含 Stage 1 至 Stage 5 的規劃）。
*   `README.md`：系統架構與操作手冊。
*   `requirements.txt`：Python 相依套件清單。
*   `tools.py`：（待實作）自訂工具庫，供 ReAct Agent 使用。
*   `simple_agent.py`：（待實作）ReAct 核心循環邏輯。

### 2. 重要機制與開發規範
*   **字幕清理規則**：
    1. 移除 VTT 元資料檔頭與時間戳記（如 `00:00:01.000 --> 00:00:03.000`）。
    2. 移除 HTML/XML 標籤。
    3. **滾動式去重**：YouTube 自動字幕會有重複的滾動行，需實作連續行去重演算法，生成流暢的段落。
    4. 輸出為 Markdown，將影片標題作為 H1，並包含原始 YouTube 連結。
*   **記憶與知識維護**：
    *   `Clippings/` 下的原始字幕檔不可直接修改。
    *   所有的知識庫整理需輸出至 `知識庫/`，並主動更新其 `index.md` 與 `log.md`。

---

## 部署與版本控制
*   **分支**：`main`（預設）
*   **儲存庫**：GitHub 公開儲存庫 `changyiwu/sensebar-agent-knowledge-vault-builder`。

