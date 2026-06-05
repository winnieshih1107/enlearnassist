# 📝 ESL 課堂對話紀錄與修正

> 英文課逐字稿智慧整理工具 — 自動修正語句、產生練習清單、TTS 音檔練習

🔗 **線上使用：** https://winnieshih1107.github.io/enlearnassist/

---

## 功能特色

| 功能 | 說明 |
|------|------|
| 📥 逐字稿匯入 | 貼上純文字逐字稿，自動識別 Teacher / Student 角色 |
| 🤖 AI 批量修正 | 一鍵分析所有學生句子，自動標示錯誤與正確用法 |
| 📋 Claude HTML 匯入 | 直接匯入 Claude.ai 回傳的修正 HTML，一次建立完整課堂紀錄 |
| ✎ 手動修正 | 對每句台詞手動標記錯誤語句、輸入正確說法與中文說明 |
| 🔊 TTS 練習 | 練習清單支援真人語音播放（Brian / Amy / Joey / Joanna） |
| ⚡ 語速調整 | 0.7x / 1.0x / 1.3x / 1.6x 四段語速 |
| ✅ 練習進度 | 打勾標記已完成，進度條即時更新 |
| 📤 匯出 | 一鍵匯出練習清單為 TSV 格式（支援 Excel 開啟） |

---

## 使用流程

### 方法 A：Claude HTML 匯入（推薦）

這是最完整的工作流程，可以一次建好整份課堂紀錄含所有修正。

**Step 1 — 準備提示詞**

將以下提示詞連同課堂逐字稿一起貼到 [Claude.ai](https://claude.ai)：

```
請將以下 ESL 課堂對話整理成修正格式，學生：Winnie
使用以下 HTML class 輸出：
- .section-title：段落標題（如「詞彙學習 Vocabulary」）
- .exchange > .speaker-t：Teacher 標籤
- .exchange > .speaker-s：Student 標籤
- .line：每行對話內容
- .error（紅色劃掉）：學生說錯的部分
- .fix（綠色）：正確說法
- .note：中文說明（一句話）

逐字稿：
[貼上逐字稿]
```

**Step 2 — 複製 Claude 回傳的 HTML**

Claude 回傳修正格式 → 全選複製

**Step 3 — 匯入工具**

點工具頂部「**📥 Claude HTML 匯入**」→ 貼上 HTML → 點「解析並建立課堂」

→ 工具自動建立課堂、對話與所有修正，顯示如下格式：

```
Winnie
The patient has a serious disease, so ~~they will die quickly~~ they may not recover.
  │ 「die quickly」語感不精確，terminally ill 指終將致死，不一定是短期內。
```

---

### 方法 B：貼上逐字稿 + AI 批量修正

適合沒有 Claude.ai 帳號，或想在工具內直接分析的情境。

**Step 1 — 貼上逐字稿**

點頂部「**貼上逐字稿**」，輸入學生姓名（如 Winnie），貼上原始逐字稿。

**Step 2 — 指定角色**

- 若逐字稿有 `Teacher:` / `Winnie:` 標籤 → 自動解析
- 若無標籤 → 彈出角色指定視窗，系統自動偵測（問句 → Teacher，短回應 → Student），可手動調整

**Step 3 — AI 批量修正**

點對話區底部「**🤖 AI 批量修正**」：

| 條件 | 行為 |
|------|------|
| 已設定 Claude API Key | 直接呼叫 API，自動分析並套用所有修正 |
| 未設定 API Key | 自動產生提示詞，貼到 Claude.ai 取得 JSON 後貼回套用 |

> 設定 API Key：點 ⚙ 圖示 → 輸入 Anthropic API Key（儲存在本機，不上傳）

---

### 方法 C：手動修正

適合老師自行標記修正內容。

1. 匯入逐字稿後，滑鼠移到學生台詞 → 點「**✎ 手動修正**」
2. 填寫：
   - **錯誤語句**：原文中需劃掉的片語（如 `they will die quickly`）
   - **正確用法**：更自然的說法（如 `they may not recover`）
   - **修正說明**：中文解釋（選填）
3. 點「＋ 加入此修正」→ 即時顯示在對話記錄中

---

## 練習清單使用方式

| 操作 | 說明 |
|------|------|
| ＋ 加入練習 | 將句子（已套用修正的正確版）加入練習清單 |
| ▶ 播放 | TTS 朗讀該句子 |
| ↺3（手動修正視窗）| 重複播放 3 次強化記憶 |
| ▶ 全部播放 | 依序播放所有待練習句子 |
| ☑ 打勾 | 標記為已完成 |
| 匯出 | 下載 .tsv 練習清單（可用 Excel / Google Sheets 開啟） |

---

## 自動角色偵測邏輯

匯入無標籤逐字稿時，系統依下列規則自動判斷：

| 條件 | 判定 |
|------|------|
| 句末有 `?` | Teacher |
| 問句的下一行 | Student |
| Yes / No / Okay / Sorry / I see 等短回應 | Student |
| `I'm teacher...` | Teacher |
| `I'm [學生名字]` | Student |
| 系統訊息（conference / internet / connection）| 略過 |

---

## 技術架構

- **開發語言：** 純 HTML / CSS / JavaScript（無框架依賴）
- **語音合成：** StreamElements TTS API（雲端）+ Web Speech API（備用）
- **語法分析：** LanguageTool 免費 API（🤖 自動修正功能）
- **AI 修正：** Anthropic Claude API（選用，需自備 API Key）
- **資料儲存：** localStorage（所有課堂紀錄存於本機瀏覽器）
- **HTML 解析：** DOMParser 解析 Claude HTML 輸出

---

## 資料說明

- 所有課堂紀錄儲存於**本機瀏覽器 localStorage**，換裝置不會同步
- 清除瀏覽器快取將**永久刪除**所有紀錄，建議定期點「匯出」備份
- Claude API Key 僅儲存於本機，不會傳送至任何第三方伺服器

---

## 授權

MIT License — 自由使用與修改
