# 數位葡萄園 (DigiVineyard) 網站更新與部署指南

這份指南記錄了本站（Hugo + Stack 主題）推送到 GitHub 與 Cloudflare Pages 自動部署的標準流程與常見排錯步驟。

---

## 核心部署架構

```
[本機編輯 (Obsidian / VS Code)]
              ↓
    git add . & git commit
              ↓
       git push master
              ↓
  [GitHub 儲存庫 (777tycoon/Ianshih)]
              ↓ (Webhook 自動觸發)
 [Cloudflare Pages 自動建置與發布]
              ↓
    https://digivineyard.com/
```

- **儲存庫位置**：`git@github.com:777tycoon/Ianshih.git` (分支: `master`)
- **主機平台**：Cloudflare Pages (非 Netlify)
- **注意**：專案中的 `public/` 資料夾已被 `.gitignore` 排除，推送到 GitHub 的是原始碼，靜態網頁由 Cloudflare 雲端直接編譯生成。

---

## 標準發布三部曲（日常更新）

每次寫好新文章或改動主題設定後，打開 PowerShell / 終端機，執行以下標準步驟：

```powershell
# 1. 切換至專案根目錄
cd D:\Obsidian\DigiVineyard

# 2. 將所有修改與新檔案加入暫存區
git add .

# 3. 提交變更並填寫更新說明
git commit -m "更新文章與樣式"

# 4. 推送到 GitHub
git push origin master
```

> **成功標誌**：第四步執行後，終端機出現 `Writing objects: 100% ...` 且最後顯示 `master -> master`，代表已成功送達 GitHub。約 1~2 分鐘後 Cloudflare 就會發布完成。

---

## 常見情境與排錯

### 情境 A：執行 `git push` 出現 `Everything up-to-date` 但網站沒更新？
* **原因**：尚未執行 `git add .` 與 `git commit`，修改只存在本機硬碟，尚未進入 Git 版本庫。
* **解法**：確實依序執行 `git add .` → `git commit -m "..."` → `git push origin master`。

---

### 情境 B：推送時被拒絕（遠端有新版本）
因為 GitHub Actions 設定了每天自動排程檢查並更新 Hugo Stack 主題 (`.github/workflows/update-theme.yml`)，有時遠端會比本機多出主題更新的 commit。

若直接 push 被拒絕，請依以下順序處理：

```powershell
# 先將遠端更新拉回並重新基準化 (Rebase)
git pull origin master --rebase

# 若無衝突，直接推送
git push origin master
```

> **注意**：若執行 pull 時出現 `error: cannot pull with rebase: You have unstaged changes`，代表本機有未存檔的變更。請**先執行 `git add .` 與 `git commit`**，存檔後再執行 `git pull origin master --rebase`，最後 `git push origin master`。

---

### 情境 C：本機即時預覽確認（發布前測試）
如果你想在推送前先在電腦上看效果：

```powershell
# 啟動 Hugo 本地預覽伺服器 (包含草稿文章)
hugo server -D
```
開啟瀏覽器訪問 `http://localhost:1313/`，確認排版與內容無誤後，按 `Ctrl + C` 結束伺服器，再執行標準推送流程。

---

## Cloudflare 後台必要環境變數檢查清單

若網站發布失敗（Build Failed），請至 **Cloudflare Dashboard → Workers & Pages → 專案名稱 → Settings → Build & deployments** 確認：

| 設定項目 | 正確數值 | 說明 |
| :--- | :--- | :--- |
| **Framework preset** | `Hugo` | 預設框架模板 |
| **Build command** | `hugo --gc --minify` | 編譯指令 |
| **Build output directory** | `public` | 發布目錄 |
| **環境變數: `HUGO_VERSION`** | `0.134.0` 或更高 | **關鍵**：未設定可能導致舊版 Hugo 編譯 Stack 主題失敗 |
| **環境變數: `TZ`** | `Asia/Taipei` | 確保文章時間與時區正確 |
