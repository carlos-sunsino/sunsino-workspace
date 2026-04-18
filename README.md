# Sunsino Workspace

Sunsino 創投生態平台的產品開發工作空間，彙整雙平台（DealFound、STARTUP101）的需求文件、架構設計、技術筆記與共用規範。

## 目錄結構

| 目錄 | 說明 |
| --- | --- |
| [00-inbox/](./00-inbox/) | 待整理的輸入文件，包含過往的 BRD、MRD 與市場研究 |
| [01-local-setup/](./01-local-setup/) | 本機開發環境設定指引 |
| [02-notes/](./02-notes/) | 技術研究與設計筆記 |
| [shared/sunsino-ssot](./shared/sunsino-ssot/) | 正式文件庫（submodule）：願景、計畫、功能規格、架構決策 |
| [shared/sunsino-prompts](./shared/sunsino-prompts/) | Claude 提示詞與規範集（submodule） |
| [shared/sunsino-prototype](./shared/sunsino-prototype/) | 前端原型 monorepo（submodule）：DealFound、STARTUP101、Sunsino 三支 Vue 應用及共用 UI 元件庫 |

## 雙平台架構

```mermaid
graph LR
    A[STARTUP101\n新創募資媒合平台] -->|優質案源輸送| B[DealFound\nSPV 投資評估平台]
    B -->|投資人回流| A
```

- **STARTUP101**：新創生態圈入口，解決資訊不對稱與媒合效率問題
- **DealFound**：AI 驅動 SPV 投資平台，提供專業評估與資產管理服務

## 正式文件

詳細的需求規格、架構設計與產品規劃請參閱 [shared/sunsino-ssot](./shared/sunsino-ssot/)。

## Submodules

本工作空間包含三個 git submodule：

```bash
# 初始化並拉取 submodule
git submodule update --init --recursive
```
