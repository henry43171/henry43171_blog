# Henry Automates - Personal Tech Blog

個人技術部落格與作品集網站，紀錄自動化測試、工具開發與程式設計經驗。

* **網站網址**: https://henry43171.github.io/
* **建置工具**: Hugo (Theme: PaperMod)
* **部署方式**: GitHub Actions

---

## 本地開發與寫作流程

### 1. 新增文章
使用 Hugo CLI 建立新文章：
~~~bash
hugo new post/new_article/index.md
~~~
> - 建立形式：以建立資料夾和 index.md 為主，方便後續新增圖片以及維護
> - 草稿/正式切換：寫作完畢準備發布前，請確保文章頂部的 Front Matter 設為 draft: false

### 2. 啟動本地預覽伺服器
進入專案目錄後執行：
```bash
hugo server -D --port=2434
```

開啟瀏覽器訪問 http://localhost:2434/ 即可即時預覽，固定 port 會比較方便檢查，port 可自行替換。

### 3. 部署發布至網站

~~~bash
git add .
git commit -m "update new post"
git push origin master
~~~

## 常用專案指令與清理
清理本地編譯快取:
public, resources 資料夾，如果有替換主題、頻繁執行 hugo 指令等操作，會產生很多殘餘檔案，可以手動清掉。

~~~bash
# 用 PowerShell 執行
# Remove-Item -Recurse -Force public, resources -ErrorAction SilentlyContinue

hugo mod clean
~~~