# glm-analysis：廣義線性模型（GLM）分析 Skill

一個給 Claude 使用的 Skill，用 R 帶您走完一套完整的 GLM 分析流程。每個階段都會逐步呈現結果，並用白話解釋統計意涵，不是只丟一堆輸出。

## 這個 Skill 在做什麼

當您提供資料並提出「幫我做 GLM」「邏輯斯迴歸」「卜瓦松迴歸」「這筆資料適合做什麼迴歸」這類需求，Claude 會依序進行四個階段：

| 階段 | 內容 |
|---|---|
| 1. 探索性資料分析（EDA） | 列出所有變數並確認哪些是 X、哪些是 Y；確認研究目的與時間順序；描述性統計與圖表；檢查最大最小值、缺失值、可能的 typo |
| 2. 適合性評估 | 判斷資料適不適合用 GLM（觀測是否獨立、是否需要 offset、是否有完全分離等） |
| 3. 模型配適 | 依反應變數型態挑選 family 與 link，檢查連續變數的函數形式，系統性比較候選模型，得到最終模型並解釋係數 |
| 4. 模型診斷與評估 | 殘差、過度離散、共線性（VIF／GVIF）、影響力觀測值（Cook's distance）、適合度、樣本外預測評估，最後讓結論與最終模型一致 |

### 支援的分布家族

| 反應變數型態 | 初步候選 family | 常用 link |
|---|---|---|
| 單筆二元 0/1 | binomial | logit（亦可 probit、cloglog） |
| 成功數／失敗數 | binomial（cbind） | logit |
| 非負整數計數 | poisson | log |
| 計數且過度離散 | quasipoisson 或 MASS::glm.nb | log |
| 連續、嚴格為正 | Gamma | log 或 inverse |
| 連續、大致對稱 | gaussian | identity |

這些只是建立候選模型的起點。Skill 的設計原則是**診斷指標是證據，不是自動決策規則**：VIF 大於 5、Cook's distance 大於 4/n、離散比值大於 1 這類數字都只當警示，會搭配資料結構與研究目的一起判讀。

### 範圍限制

本 Skill **不支援**下列情況，遇到時只會判斷並說明，然後建議改用專門工具：

- 零膨脹模型（zero-inflated）與門檻模型（hurdle）：只說明兩者的差異與如何區分，不做配適與解釋
- 重複測量、群聚資料（GLMM、GEE）
- 連續比例資料（beta regression）

## 如何對話

直接用自然語言描述需求，並附上資料（csv、xlsx 等）。不需要說出「GLM」這三個字。

範例：

> 我有一份資料 `visits.csv`，想知道哪些因素會影響看診次數，幫我做廣義線性模型分析。

> 這筆資料的 Y 是 0/1，幫我做邏輯斯迴歸，並用 Quarto 產生 PDF 報告。

> 我已經做完 EDA 了，直接幫我從模型配適開始。

### 三種工作模式

| 模式 | 說明 |
|---|---|
| 完整分析 | 從 EDA 一路做到診斷，每個階段都停下來解釋 |
| 指定步驟 | 只做您指定的階段，例如「只做診斷」 |
| 接續分析 | 您已有部分結果，從中間接手 |

### 流程中 Claude 會問您的事

1. **哪個是反應變數（Y）、哪些是解釋變數（X）**：如果您沒說明，Claude 會先列出所有變數再詢問
2. **研究目的與時間順序**：是描述關聯、因果推論還是預測？變數的時間先後為何？
3. **資料是否有問題**：EDA 做完後，Claude 會列出各變數的最大最小值與可疑數值，請您確認有沒有 typo 或怪值，您回覆沒問題才會繼續
4. **是否有暴露量不同**：計數資料若觀察期間或人口數不同，會詢問是否需要 offset

## 輸出方式：對話內，或 Quarto 報告

可以依需求二選一。

### 選項一：直接在對話中輸出

預設做法。結果表格、圖與解釋直接出現在對話裡。除非您主動要求，否則**不會貼出 R 程式碼**，避免干擾閱讀。

### 選項二：產生 Quarto 報告檔

在需求中說明「要報告」「要 PDF」「要 HTML」「要 Word」即可。Skill 附有通用範本 `assets/report_template.qmd`，同一份 qmd 可輸出三種格式：

| 格式 | 渲染指令 |
|---|---|
| PDF | `LC_ALL=C.UTF-8 quarto render report.qmd --to pdf` |
| HTML | `LC_ALL=C.UTF-8 quarto render report.qmd --to html` |
| Word | `LC_ALL=C.UTF-8 quarto render report.qmd --to docx` |

範本特色：

- 支援繁體中文（PDF 使用 xelatex 與 xeCJK，字型自動依序嘗試）
- 圖表用 `fig-` / `tbl-` 標籤，可交叉引用
- 章節自動編號、目錄、`cache` 加速
- 報告章節與四個分析階段一一對應

## 分析在哪裡執行

分析是用 **R** 跑的。依您使用的環境而定：

- **在 Claude 的雲端環境**：Claude 會先檢查 R 是否存在，沒有就安裝（R、MASS、car、ggplot2、dplyr、performance、knitr、rmarkdown、broom，PDF 另需 Quarto 與 LaTeX 相關套件），您的電腦不需要有 R
- **在自己的電腦（Claude Code 等）**：使用您本機的 R 與 Quarto

若環境完全無法執行 R，Skill 規定必須明說，**不可假裝已經跑過分析**。

### 如何確認結果是真的跑出來的

最可靠的做法是重現：

1. 要求 Claude 提供 `.qmd` 或 `.R` 檔，並附上您的資料
2. 在自己的 R／RStudio 重新執行（交叉驗證等隨機步驟有固定種子）
3. 對照數字是否一致

## 安裝方式

### Claude.ai（網頁或桌面版）

1. 到 Releases 下載 `glm-analysis.skill`（或自行把 `glm-analysis` 資料夾壓縮成 zip）
2. 在 Claude 的設定中找到 Skills 並上傳

### Claude Code

把整個 `glm-analysis` 資料夾放到：

```
~/.claude/skills/glm-analysis/
```

專案內使用則放在專案的 `.claude/skills/glm-analysis/`。

## 資料夾結構

```
glm-analysis/
  SKILL.md                              主要指示：工作模式、四個階段的流程與原則
  references/
    family-link-selection.md            依反應變數型態選 family 與 link
    diagnostics-checklist.md            模型診斷清單與判讀注意事項
    model-building-and-validation.md    研究目的、函數形式、候選模型比較、交叉驗證
  assets/
    report_template.qmd                 Quarto 報告範本（pdf、html、docx 通用）
```

`SKILL.md` 會在觸發時載入，`references/` 與 `assets/` 的內容則在需要時才讀取。

## 設計原則摘要

- 先確認研究問題與時間順序，再談模型
- 每個連續變數都檢查函數形式，不預設線性
- 候選模型有系統地比較，且在相同的分析樣本上比較
- 診斷（模型有沒有問題）與預測評估（模型預測得準不準）分開處理
- 結論必須與最終模型一致，並報告實際使用的樣本數
- 係數解釋要寫清楚參考組、單位與尺度；odds ratio 不等於風險比

## 限制與提醒

- 這個 Skill 輔助分析流程，不能取代統計專業判斷。重要的研究結論請自行確認
- 模型選擇、變數處理方式會影響結論，Claude 會說明理由，但最終決定在您
- 資料若含個資，上傳前請先去識別化
