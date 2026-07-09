---
title: 別再手動 Git Push！用 GitHub Actions 打造 Obsidian 一鍵發布 Hugo 部落格的自動化工作流（CI/CD）
description: 解決手動部署 Hugo 的瑣碎步驟。本文手把手教你如何配置 GitHub Actions，讓你在 Obsidian 寫完筆記後，透過一鍵自動觸發 CI/CD 流程，完成靜態網頁編譯與發布，打造零摩擦力的數位花園。
slug: obsidian-hugo-github-actions-automation
date: 2026-07-09 09:10:00+0800
image:
draft: true
categories:
  - Obsidian
  - Hugo
tags:
  - GitHubActions
  - CI/CD
  - Automation
  - Productivity
weight: 1
---
# 別再手動 Git Push！用 GitHub Actions 打造 Obsidian 一鍵發布 Hugo 部落格的自動化工作流

這篇文章將指引讀者如何利用 **GitHub Actions** 實現持續整合與部署（CI/CD），在 Obsidian 按下一個鍵，文章就自動同步到雲端並渲染上線，實現真正的「無痛寫作」。


### 文章結構與核心內容（Article Structure）

#### 1. Hook（痛點切入）
每次寫完筆記，還要打開終端機輸入 `hugo`、`git add .`、`git commit -m "update"`、`git push`？這種繁瑣的流程，光想到這些就懶得更新文章了。
真正的「數位花園（Digital Garden）」應該是：**你在 Obsidian 寫完，按下儲存，幾秒鐘後網頁就自動更新完成。**

#### 2. 架構解析：自動化發布是怎麼運作的？（How it Works）
用一張簡單的流程圖解釋：
`[Obsidian 寫作] -> [Obsidian Git 插件自動/手動 Push] -> [GitHub 接收觸發] -> [GitHub Actions 啟動虛擬機] -> [編譯 Hugo] -> [部署至 GitHub Pages / Cloudflare Pages]`

#### 3. 實作三步驟（Step-by-Step Implementation）

##### 步驟一：在 Obsidian 安裝「Git」插件
透過此插件，在手機或電腦端，一鍵將筆記同步回 GitHub 儲存庫。

##### 步驟二：編寫 GitHub Actions Workflow YAML 檔
在專案根目錄建立 `.github/workflows/deploy.yml`。提供一個實用、免踩坑的範本（已針對 Hugo Stack 主題最佳化）：

```yaml
name: Deploy to Github Pages

on:
  push:
    branches: [master] # 僅在 master 分支有新 Commit 時觸發。移除 pull_request 避免外部惡意觸發

# 僅給予 Pages 部署與讀取程式碼的必要最小權限，安全性最高
permissions:
  contents: read
  pages: write
  id-token: write

# 避免多個 Commit 同時觸發時產生衝突，自動取消先前的執行任務
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive # 如果主題是 Git Submodule，此參數必須存在
          fetch-depth: 0

      # 💡 效能優化建議：如果你的 Hugo 主題「沒有」使用 Hugo Modules (go.mod)
      # 下面這段 "Setup Go" 可以整段刪除，能幫你每次建置省下 15~30 秒。
      - name: Setup Go
        uses: actions/setup-go@v5
        with:
          go-version: "^1.21.0"

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3 # 升級至最新的 v3 版本
        with:
          hugo-version: "latest"
          extended: true

      - name: Build
        run: hugo --minify --gc

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public # 將編譯好的靜態檔案打包上傳至 GitHub 暫存區

      - name: Deploy 🚀
        id: deployment
        uses: actions/deploy-pages@v4 # 改用 GitHub 官方最新直接部署 API，不再產生 gh-pages 分支


```

##### 步驟三：設定 GitHub Repository 權限
* 到 `Settings > Actions > General > Workflow permissions`，將權限改為 `Read and write permissions`。這是 90% 新手配置 GitHub Actions 失敗（出現 403 錯誤）的主因。

#### 4. 風險與進階優化建議（Risk & Optimization）
* **草稿過濾機制（Draft Filter）**：提醒在 YAML 中善用 `draft: true`，避免半成品筆記直接暴露在公網。
* **私密筆記與公開網頁分離（Private vs. Public Vault）**：如果讀者的 Obsidian 是私有庫，如何利用 Action 只把 `content/` 資料夾中特定的公開文章推送到 Hugo 的公開庫中，兼顧隱私與分享。

### 結語
>待補充


**[風險提示 Risk Warning：自動化部署涉及 GitHub 個人存取權杖（Personal Access Token）與憑證設定，若權限開得過大或外洩，可能導致私有儲存庫（Private Repository）外流。請務必遵循最低權限原則（Principle of Least Privilege）進行設定。]**
