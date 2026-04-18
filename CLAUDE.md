# sunsino-workspace

Sunsino 雙平台（DealFound、STARTUP101）產品開發工作空間，彙整需求文件、架構設計與 AI 配置。詳見 @README.md。

## 目錄結構

- `00-inbox/` — 待整理輸入文件
- `01-local-setup/` — 本機環境設定指引
- `02-notes/` — 技術研究與設計筆記
- `shared/sunsino-ssot/` — 正式文件庫（submodule）
- `shared/sunsino-prompts/` — Claude 提示詞與規範集（submodule）
- `shared/sunsino-prototype/` — 前端原型 monorepo（submodule）

## 規範

完整規範已拆分至 `.claude/rules/`，按需自動載入：

- 架構：`arch-ddd-strategic.md`、`arch-ddd-tactical.md`、`arch-layering.md`、`arch-dependency.md`
- 程式碼：`code-style.md`、`code-naming.md`、`code-typescript.md`、`code-testing.md`
- 前端：`frontend-component.md`、`frontend-state.md`
- 資料：`data-modeling.md`
- API：`api-design.md`、`spec-api.md`
- 文件：`doc-annotation.md`、`doc-writing-style.md`

## 禁止事項

- 禁止直接修改 submodule 目錄內的檔案——進入對應 submodule repo 操作
- 禁止在工作空間根目錄建立 `.env` 或含憑證的設定檔
