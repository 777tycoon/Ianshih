---
title: Hugo 重安裝心得
description: 在電腦上打開文章
slug: hugo-reinstall-feeling
date: 2026-01-14 16:03:54+08:00
image:
categories:
  - Example Category
tags:
  - Example Tag
weight: 1
---
### 主題選擇：從 Hextra 到簡單路線

原本很想用 Hextra 這個主題，作者更新超勤快，功能也很強大。但最近一次拉更新後，設定檔結構變了不少，我反而看不懂怎麼改了（可能是我太久沒碰）。 最後還是決定回歸簡單路線——畢竟我做這個 Blog 的初衷，只是想有一個地方紀錄自己的思考，不需要太複雜的花俏功能。找個容易上手的主題，專心寫內容就好。

### 部署過程：Netlify 與 Cloudflare 的來回

第一次部署選了 Netlify，因為很多教學文都是用它。過程遇到不少坑（權限、build 設定、branch 連動等），還好有 AI 幫忙一步步 debug，最終在 2026/01/14 成功上線！ 那一刻真的很有成就感，也讓我對靜態網站部署更有信心。

後來又爬了一些文，發現 Cloudflare Pages 其實更適合我的使用情境（免費額度大、速度快、整合 Git 超方便）。

我之前有 Cloudflare 架站經驗，就決定遷移過去。 雖然一開始有卡住——我錯點了「Workers」的建立按鈕，用了很久才發現。 正確做法是：

1. 進到 Cloudflare 儀表板
2. 不要直接點中間大藍色「Create application」按鈕
3. 而是要點上方小小的「+」圖示 → 選擇「Pages」 → Connect to Git
4. 綁定 GitHub 儲存庫，設定 build command 為 hugo --minify，publish directory 為 public

照著做之後就順利部署了。Cloudflare 的介面比想像中更直覺，build 速度也很快。

### 心得與小提醒

這次從頭重裝 Hugo、換主題、來回試兩個平台，雖然花了不少時間，但收穫很大。現在對靜態網站的整個流程（從本地測試、git push 到自動部署）已經更熟悉了。 如果你也想用 Hugo 建個人數位花園或思考記錄站，強烈建議從簡單主題開始，部署時多用 AI 輔助 debug，真的會省下很多時間。

未來我會繼續用這個網站記錄投資、閱讀與生活心得，但會注意內容尺度，避免寫太多私人或工作相關的細節。