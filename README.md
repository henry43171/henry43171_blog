# Henry Automates - Personal Tech Blog

個人技術部落格與作品集網站，紀錄自動化測試、工具開發與程式設計經驗。

* **網站網址**: https://henry43171.github.io/
* **建置工具**: Hugo (Theme: PaperMod)
* **部署方式**: GitHub Actions

---

## 本地開發與寫作流程

### 1. 啟動本地預覽伺服器
進入專案目錄後執行：
```bash
hugo server -D
```

開啟瀏覽器訪問 http://localhost:2434/ 即可即時預覽（支援存檔自動刷新）。

### 2. 新增文章
使用 Hugo CLI 建立新文章：
~~~bash
hugo new posts/your-post-title.md
~~~
> 備註：寫作完畢準備發布前，請確保文章頂部的 Front Matter 設為 draft: false。

### 3. 部署發布至網站

~~~bash
git add .
git commit -m "update new post"
git push origin master
~~~

## 常用專案指令與清理
清理本地編譯快取:

~~~bash
Remove-Item -Recurse -Force public, resources -ErrorAction SilentlyContinue
hugo mod clean
~~~