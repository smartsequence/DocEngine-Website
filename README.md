# 智序資訊工作室官網

智序資訊工作室（SmartSequence Tech Studio）官方網站，使用 Astro 框架建立，部署在 Vercel。

## 🌐 網站架構

- **官網**: https://smartsequence.tech
- **ForgeHelm SaaS**: https://app.forgehelm.com
- **ForgeHelm API**: https://api.forgehelm.com

## 🚀 技術棧

- **框架**: [Astro](https://astro.build/) - 現代化靜態網站生成器
- **樣式**: [Tailwind CSS](https://tailwindcss.com/) - 實用優先的 CSS 框架
- **部署**: [Vercel](https://vercel.com/) - 極速部署 + 全球 CDN
- **表單**: Web3Forms - 免費表單服務

## 📁 專案結構

```
SmartSequence-Website/
├── .github/
│   └── workflows/
│       └── azure-static-web-apps.yml    # CI/CD 配置
├── src/
│   ├── pages/                           # 頁面
│   │   ├── index.astro                  # 首頁
│   │   ├── features.astro               # 功能特色
│   │   ├── pricing.astro                # 定價方案
│   │   ├── contact.astro                # 聯絡我們
│   │   ├── about.astro                  # 關於我們
│   │   └── 404.astro                    # 404 頁面
│   ├── components/                      # 元件
│   │   ├── Header.astro                 # 頁首
│   │   ├── Footer.astro                 # 頁尾
│   │   └── BaseHead.astro               # HTML Head
│   ├── layouts/                         # 佈局
│   │   └── BlogPost.astro               # 部落格文章佈局
│   └── styles/                          # 樣式
│       └── global.css                   # 全域樣式
├── public/                              # 靜態資源
│   ├── favicon.ico
│   └── robots.txt
├── staticwebapp.config.json            # Azure Static Web Apps 配置
├── astro.config.mjs                    # Astro 配置
└── package.json                        # 依賴管理
```

## 🧞 開發指令

所有指令都在專案根目錄執行：

| 指令 | 說明 |
| :--- | :--- |
| `npm install` | 安裝依賴套件 |
| `npm run dev` | 啟動開發伺服器 (localhost:4321) |
| `npm run build` | 建置生產版本到 `./dist/` |
| `npm run preview` | 預覽建置結果 |
| `npm run astro ...` | 執行 Astro CLI 指令 |

## 🔧 開發流程

### 本地開發

```bash
# 安裝依賴
npm install

# 啟動開發伺服器
npm run dev

# 瀏覽器開啟 http://localhost:4321
```

### 建置與預覽

```bash
# 建置生產版本
npm run build

# 預覽建置結果
npm run preview
```

## 🚢 部署流程

### 自動部署（Vercel）

每次推送到 `main` 分支時，Vercel 會自動：

1. ✅ 檢測變更
2. ✅ 建置專案
3. ✅ 部署到全球 Edge Network
4. ✅ 更新生產環境（< 1 分鐘）

### Pull Request 預覽

每個 Pull Request 會自動建立預覽環境：
- 預覽 URL: `https://xxx-preview.vercel.app`
- PR 合併或關閉後自動清理

## 🌍 相關專案

- [ForgeHelm-SaaS](https://github.com/smartsequence/ForgeHelm-SaaS) - 主應用程式
- [ForgeHelm-Agent](https://github.com/smartsequence/ForgeHelm-Agent) - 客戶端 Agent
- [ForgeHelm-Contracts](https://github.com/smartsequence/ForgeHelm-Contracts) - 通訊協議

## 📝 文件

詳細的架構規劃請參考：
- [網站架構規劃](./SITE_ARCHITECTURE.md) - 完整頁面架構與內容規劃
- [部署方案比較](./DEPLOYMENT_RECOMMENDATION.md) - Azure vs Vercel 比較
- [GitHub 設定指南](./GITHUB_SETUP_GUIDE.md) - Repository 建立步驟

## 🔒 安全性

- ✅ 自動 HTTPS (Let's Encrypt)
- ✅ 安全標頭配置 (CSP, HSTS, X-Frame-Options)
- ✅ DDoS 防護 (Azure 內建)
- ✅ 內容安全策略

## 📊 效能

- ⚡ Lighthouse 分數: 100/100
- ⚡ 頁面載入時間: < 3 秒
- ⚡ 全球 CDN 加速
- ⚡ 自動圖片優化

## 📄 授權

Copyright © 2026 SmartSequence Tech. All rights reserved.

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request！

---

**建立日期**: 2026-01-25  
**維護者**: SmartSequence Tech Team
