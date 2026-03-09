---
title: Obsidian 筆記轉換 Hugo :解決 WikiLinks 內部連結失效 (Internal Links Fix)
description: 解析如何透過 Hugo 遮蔽機制 (Lookup Order) 修復 Obsidian 的 [[雙括號]] 語法，解決靜態網頁渲染 404 錯誤，並保持佈景主題的原生完整性。
slug: obsidian-hugo-wikilinks-fix-guide
date: 2026-03-08 22:50:42+08:00
image: cover.webp
categories:
  - Obsidian
  - Hugo
tags:
weight: 1
---


## 建置 Hugo 網站後難關才剛開始

眾所周知 Obsidian 是絕佳的思考與寫作空間，而 Hugo 則是將這些文章轉為公開「數位花園」的首選工具。然而，當我試圖將兩者結合時，遇到的第一個大挑戰就是語法的不相容，特別是 Obsidian 最強的功能內部連結 (internal links) ， APP 裡 Markdown 語法是這樣寫的 ```[ [ 文字 ] ] ```

但在標準 Markdown 語法中，Obsidian 的 `[ [ 文字 ] ]`  語法在並不被承認，導致 Hugo 渲染出的網頁只會顯示一串無意義的括號或只能點擊但無反應的底線字。


### 如何改善
為了在網頁生成前攔截內容，將 `[ [ 文字 ] ]` 轉換為 HTML 的 `<a href="...">` 標籤。這一步將讓靜態網頁具備網頁連結。
### 模組化陷阱
起初 AI 建議我加一個 `single.html`，結果導致整個佈景主題崩潰出現白畫面，原有的主題不見(但 Hugo 的好處是所見即所得，僅在 local 端展示而已)。

這次經驗讓我理解了 Hugo 的「遮蔽機制」（Lookup Order）：我們不應該動到 `themes/` 資料夾裡的原始碼，而是在根目錄的 `layouts/` 中建立對應路徑。最終，我們重新將檔案放到 `layouts/partials/article/components/content.html`

這樣只針對「文章內容」進行加工，而保留了原有的佈景主題。

### 排除萬難：路徑、鎖定與美化
技術實作中總伴隨著瑣碎的坑。例如，Hugo 預設會將網址轉為小寫，而 Obsidian 的連結可能包含大寫與空格，這將導致了頻繁的 404 錯誤。透過將連結路徑統一指向 `/p/` 並建議使用 `slug` 屬性。

最後，透過在模板中注入自定義 CSS，我為這些內部連結加上了虛線底線與專屬顏色，便大功告成。

# 結語🍷：內容管理的長久之道

現在，我的 Hugo 網站終於能正確顯示連結，這套流程打破了「私密筆記」與「公開發佈」之間的牆，讓知識的流動變得更加順暢。對於每一位想要經營數位花園的人來說，這段折騰的過程，正是打磨寫作工具的必經之路。

<!--more## 📺 影片觀看 (YouTube)
{{< youtube id="你的影片ID" >}}-->


> **版權聲明：** 本文原載於 [Digital Vineyard](https://digivineyard.com)，轉載請註明出處。

