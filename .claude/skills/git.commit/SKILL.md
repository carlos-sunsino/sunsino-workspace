---
name: git.commit
description: "當使用者說「幫我 commit」、「提交變更」、「commit 這些修改」、「幫我寫 commit message」、「整理成多個 commit」、「staged 這些檔案」或需要暫存與提交時，務必使用此 skill。執行完整的 git 提交工作流程：檢視變更、拆分 commit、暫存、生成 Conventional Commits 繁中訊息並提交。不適用於僅產出 commit message 文字而不執行 git 指令的場景。"
user-invocable: true
disable-model-invocation: false
---

# Git Commit 工作流程

## 目標

建立易於審閱、安全可部署的 commit：

- 只包含預期的變更
- 邏輯上有清楚範圍（必要時拆分成多個 commit）
- 提交訊息清楚描述什麼改變了，以及為什麼

---

## 步驟 1：檢視工作樹

```bash
git status
git diff
```

若變更龐大，先看摘要：

```bash
git diff --stat
```

**目標：** 清楚了解所有 unstaged 與 staged 的變更，再決定如何分組。

---

## 步驟 2：決定 commit 邊界

判斷是否需要拆分。若存在以下不相關的變更，建議各自獨立成一個 commit：

- 功能新增 vs. 重構
- 前端 vs. 後端
- 格式調整 vs. 邏輯變更
- 測試 vs. 正式程式碼
- 依賴套件升級 vs. 行為變更

若需要拆分，**先向使用者說明計畫**：列出準備建立幾個 commit、各自包含哪些檔案或變更，等待確認後再進行。若只有一個 commit，直接進入步驟 3。

---

## 步驟 3：暫存當前 commit 的變更

```bash
# 暫存特定檔案
git add <path>

# 同一個檔案有混合用途時，互動式選擇 hunk
git add -p

# 取消暫存
git restore --staged <path>
```

> `git add -p` 常用按鍵：`y`（暫存）、`n`（跳過）、`s`（拆分 hunk）、`e`（手動編輯）、`q`（結束）。混合變更無法用 `s` 拆分時，用 `e` 手動刪除不屬於此 commit 的 `+` 行。

---

## 步驟 4：確認暫存內容

```bash
git diff --cached
```

確認以下項目：

- 無機密資訊、API key 或 token
- 無意外殘留的 debug 日誌或 console.log
- 無不相關的格式調整混入

若發現問題，退回步驟 3 調整暫存內容。

---

## 步驟 5：撰寫提交訊息

**格式（Conventional Commits）：**

```
<type>(<scope>): <description>

<body>

<footer>
```

Type 使用標準 Conventional Commits 類型：`feat`、`fix`、`docs`、`style`、`refactor`、`perf`、`test`、`build`、`ci`、`chore`、`revert`。

### 撰寫規則

- **Description**：使用繁體中文，祈使語氣（「新增」「修正」「更新」「實作」）
- **技術識別符保持原文**：套件名稱、函式名、檔案路徑、類別名、CLI 指令、API 路由等**不翻譯**
- **Description 限制 50 字元以內**，結尾不加句號
- **Body**：使用條列式（`-`），格式為 `標題: 做了什麼（為什麼）`。標題是變更的核心動詞短語，括號內補充動機或背景。每條聚焦一件事，讓讀者一眼知道改了什麼並理解為何這樣做
- **Footer**：有重大變更時加上 `BREAKING CHANGE: 說明`；有關聯 issue 時加上 `Closes #123`

### 範例

```
feat(auth): 新增 JWT 令牌驗證機制

- 無狀態驗證: 改用 jsonwebtoken 取代 session-based 做法（支援無伺服器部署，不依賴 server-side session 儲存）

BREAKING CHANGE: /api/login 回傳格式由 session cookie 改為 Bearer token
```

```
fix(ui): 修正 Button 元件在 Safari 上的對齊問題
```

```
chore(deps): 升級 react 至 18.3.0
```

```
refactor(api): 將 getUserById 抽離至獨立的 service 層

- 抽離 UserService: 透過 UserService.findById() 統一存取（原本邏輯直接散落在 controller，導致難以單元測試）
- 統一存取界面: 便於 mock 與替換，降低 controller 與資料層的耦合
```

---

## 步驟 6：呈現提交內容並執行

簡短列出以下資訊供使用者知悉，然後直接執行提交（無需等待確認）：

1. 此 commit 包含的檔案清單（來自 `git diff --cached --name-only`）
2. 完整的 commit 訊息（包含 body 與 footer）

執行：

```bash
# 單行訊息
git commit -m "type(scope): description"

# 含 body / footer 的多行訊息
git commit -m "type(scope): description

body 內容

footer 內容"
```

---

## 步驟 7：重複直到工作樹乾淨

若此次規劃了多個 commit，回到步驟 3，暫存下一組變更，重複直到所有預期的變更都已提交。

最後執行 `git status` 確認工作樹狀態，回報使用者最終結果。

---

## 安全守則

- 絕不修改 git config——可能影響全域設定或其他專案的行為
- 未明確要求時，絕不執行破壞性指令（`--force`、`git reset --hard`）——可能造成不可復原的歷史遺失
- 未明確要求時，絕不略過 hook（`--no-verify`）
- 絕不 force push 至 main／master
- 若 commit 因 hook 失敗，修正問題後建立全新 commit，不使用 `--amend` 修改已提交的歷史
