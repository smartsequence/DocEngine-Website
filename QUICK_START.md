# DocEngine-Website 快速啟動指南

## 🚀 本地開發

### 安裝依賴
```bash
npm install
```

### 啟動開發伺服器
```bash
npm run dev
```
開啟瀏覽器訪問：http://localhost:4321

### 建置生產版本
```bash
npm run build
```

### 預覽建置結果
```bash
npm run preview
```

## 📤 推送到 GitHub

### 首次推送
```bash
# 1. 在 GitHub 建立 repository: smartsequence/DocEngine-Website

# 2. 添加遠端倉庫
git remote add origin https://github.com/smartsequence/DocEngine-Website.git

# 3. 重命名分支為 main
git branch -M main

# 4. 推送程式碼
git push -u origin main
```

### 後續推送
```bash
git add .
git commit -m "Your commit message"
git push
```

## ☁️ 部署到 Azure

詳細步驟請參考：[DEPLOYMENT.md](./DEPLOYMENT.md)

### 簡要步驟
1. 推送程式碼到 GitHub
2. 在 Azure Portal 建立 Static Web App
3. 連接 GitHub Repository
4. 等待自動部署完成
5. 配置自訂域名（可選）

## 📝 開發頁面

### 建立新頁面
在 `src/pages/` 目錄下建立新的 `.astro` 文件：

```astro
---
// src/pages/new-page.astro
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import { SITE_TITLE, SITE_DESCRIPTION } from '../consts';
---

<!doctype html>
<html lang="zh-TW">
  <head>
    <BaseHead title={`頁面標題 - ${SITE_TITLE}`} description={SITE_DESCRIPTION} />
  </head>
  <body>
    <Header />
    <main>
      <h1>頁面標題</h1>
      <p>頁面內容</p>
    </main>
    <Footer />
  </body>
</html>
```

訪問：http://localhost:4321/new-page

### 建立新元件
在 `src/components/` 目錄下建立新的 `.astro` 文件：

```astro
---
// src/components/MyComponent.astro
interface Props {
  title: string;
}

const { title } = Astro.props;
---

<div class="my-component">
  <h2>{title}</h2>
</div>

<style>
  .my-component {
    padding: 1rem;
  }
</style>
```

使用元件：
```astro
---
import MyComponent from '../components/MyComponent.astro';
---

<MyComponent title="Hello World" />
```

## 🎨 使用 Tailwind CSS

Tailwind CSS 已經整合，可以直接使用：

```astro
<div class="bg-blue-500 text-white p-4 rounded-lg">
  Hello Tailwind!
</div>
```

## 📚 更多資源

- [Astro 文檔](https://docs.astro.build/)
- [Tailwind CSS 文檔](https://tailwindcss.com/docs)
- [Azure Static Web Apps 文檔](https://docs.microsoft.com/azure/static-web-apps/)

## 🆘 需要幫助？

查看完整文檔：
- [README.md](./README.md) - 專案說明
- [DEPLOYMENT.md](./DEPLOYMENT.md) - 部署指南
- [PROJECT_SETUP_SUMMARY.md](./PROJECT_SETUP_SUMMARY.md) - 專案設定總結

---

**提示**：開發時記得經常提交程式碼到 Git！
