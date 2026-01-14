---
title: How to Post
description: 在電腦上打開文章
slug: how-post
date: 2026-01-14 16:03:54+08:00
image:
categories:
  - Example Category
tags:
  - Example Tag
weight: 1
---

要修改頁面，可以照著以下步驟操作：

### 第一步：在電腦上找到該檔案(以 **hello-world**)

1. 打開你的 **VS Code**。
    
2. 在左側的檔案瀏覽器中，展開資料夾路徑：  
    **content** > **post** > **hello-world**
    
3. 點擊打開裡面的 **index.md** 檔案。
    
    - 註：Hugo Stack 主題使用的是「Page Bundle」模式，所以每篇文章都有自己的資料夾，內容都在 index.md 裡。
        

---

### 第二步：修改內容

打開檔案後，你會看到兩部分：

#### 1. 上半部：標頭資訊 (Front Matter)

這是被 --- 包起來的部分，決定了網頁的元數據。你可以修改：

- **title**: 網頁標題。    
- **description**: 顯示在列表中的簡介。    
- **date**: 發布日期。    
- **image**: 封面圖（可以貼上 YouTube 的縮圖網址）。    
- **categories & tags**: 分類與標籤。
    

#### 2. 下半部：文章本文

在第二個 --- 之後的所有內容，就是你在網頁上看到的文字。你可以直接用 **Markdown** 語法開始寫作：

- ## 代表二級標題。
    
- **文字** 代表粗體。
    
- 直接貼上文字就是段落。
    

---

### 第三步：存檔與上傳

修改完畢後，請在 VS Code 的終端機（Terminal）輸入你已經學會的指令，讓網頁更新：

1. **加入變動**：
    
    codeBash
    
    ```
    git add .
    ```
    
2. **存檔備註**：
    
    codeBash
    
    ```
    git commit -m "修正首篇文章內容"
    ```
    
3. **推送到雲端**：
    
    codeBash
    
    ```
    git push origin master
    ```
    

---

### 💡 實戰小技巧

#### 1. 如何即時看到修改結果？

在推送到 GitHub 之前，你可以先在 VS Code 終端機輸入：

codeBash

```
hugo server
```

然後打開瀏覽器看 http://localhost:1313/p/hello-world/。你一邊改、一邊存檔，瀏覽器的畫面會立刻跳轉更新，確認沒問題後再 git push。

#### 2. 如何更換封面圖？

如果你想讓這篇文章顯示你的 YouTube 影片封面，請修改 image 欄位：

codeYaml

```
image: "https://img.youtube.com/vi/你的影片ID/maxresdefault.jpg"
```

這樣首頁的卡片就會變得非常漂亮！

#### 3. 如何刪除或新增文章？

- **刪除**：直接在 content/post/ 刪除該資料夾，然後 git push。
    
- **新增**：在 content/post/ 下新建一個資料夾（例如 my-video-1），在裡面建立 index.md，並填入標頭資訊即可。