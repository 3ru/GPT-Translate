# 🌎 Markdown 翻譯機器人
[![Maintainability](https://api.codeclimate.com/v1/badges/a13ea4f37913ba6ba570/maintainability)](https://codeclimate.com/github/3ru/gpt-translate/maintainability)
[![GPT Translate](https://github.com/3ru/gpt-translate/actions/workflows/gpt-translate.yml/badge.svg)](https://github.com/3ru/gpt-translate/actions/workflows/gpt-translate.yml)

此 GitHub Action 使用 GPT-3.5 模型將您的 Markdown 文件翻譯成多種語言。

### ⚠️**警告**
目前 OpenAI API 不免費。您需要使用 `付費帳戶` 發行的 API 金鑰才能使用此工作流程。  
<img width="387" alt="image" src="https://github.com/3ru/gpt-translate/assets/69892552/8c803edb-85ef-41ee-a4be-be52b3a30eba">

<br/>

<details><summary> GPT-4 的 README 摘要</summary>
<p>

> - 這是一個使用 GPT-3.5 將 Markdown 文件翻譯成多種語言的 GitHub Action。
> - 要使用，請在問題或拉取請求中創建一個帶有 /gpt-translate 或 /gt 的評論，指定輸入/輸出文件路徑和目標語言。
> - 翻譯後的文件將作為拉取請求（在問題上）創建，或作為新提交添加到現有拉取請求中（在拉取請求上）。
</p>
</details> 

<details><summary>🧐 目前狀態</summary>
<p>

- 此操作僅支持翻譯單個 **Markdown 文件**。

- 命令只能由具有 **寫入存儲庫權限** 的個人執行。

這些限制可防止不受信任的人濫用 API。

我正在考慮未來實現按目錄翻譯和多選功能。
</p>
</details> 

## 🔧 設置

### 存儲庫設置

#### 1. 設置 > 操作 > 一般

- 啟用 `讀取和寫入權限`
- 啟用 `允許 GitHub Actions 創建和批准拉取請求`
  ![permissions](https://user-images.githubusercontent.com/69892552/228692074-d8d009a8-9272-4023-97b1-3cbc637d5d84.jpg)

#### 2. 設置 > 密碼和變量 > 操作

- 將 [您的 API 金鑰](https://platform.openai.com/account/api-keys) (`OPENAI_API_KEY`) 設置為 secrets
  ![secrets](https://user-images.githubusercontent.com/69892552/228692421-22d7db33-4e32-4f28-b166-45b4d3ce2b11.jpg)


### GitHub Actions 工作流程設置

#### 必需的
- 將 OPENAI_API_KEY 設置為 apiKey。
- 設置 `on` 為當評論創建時觸發 (`types: [ created ]`)。
- 預先檢查 (`actions/checkout@v3`)。

#### 建議的（為了最小化不必要的運行時間）
- 配置是否僅在評論中存在 `/gpt-translate` 或 `/gt` 時運行。


👇 這是一個最小的工作流範例：
```yaml
# .github/workflows/gpt-translate.yml
name: GPT Translate

on:
  issue_comment:
    types: [ created ]

jobs:
  gpt_translate:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Run GPT Translate
        if: |
          contains(github.event.comment.body, '/gpt-translate') || 
          contains(github.event.comment.body, '/gt')
        uses: 3ru/gpt-translate@v1.0
        with:
          apikey: ${{ secrets.OPENAI_API_KEY }}
```


## 💡 用法

```
/gpt-translate [input filepath] [output filepath] [target language] 
```
您可以使用 /gt 作為 /gpt-translate 的簡寫。

1.在問題或拉取請求中創建一個帶有 `/gpt-translate` 或 `/gt` 的評論。

2.【在問題上】翻譯後的文件將作為 **拉取請求** 創建。

2.【在拉取請求上】翻譯後的文件將作為 **新提交添加到拉取請求中**。

換句話說，如果您不斷在問題上發表評論，則會不斷創建新的 PR。
如果您不斷在 PR 上發表評論，則會不斷添加新的提交到該 PR 中。

## 📝 範例
```
/gpt-translate README.md zh-TW/README.md traditional-chinese
```
將 `README.md` 翻譯成繁體中文，並將其放置在 `zh-TW` 目錄下。

## 🌐 支持的語言
GPT-3.5 可解釋的任何語言

## 📃 許可證
MIT 許可證