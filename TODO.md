# DocEngine-Website 待辦事項

## 🎯 Phase 1: 基礎建設（已完成 ✅）

- [x] 初始化 Astro 專案
- [x] 安裝 Tailwind CSS
- [x] 配置 Azure Static Web Apps
- [x] 設定 CI/CD (GitHub Actions)
- [x] 建立基礎文檔
- [x] 建立 404 頁面
- [x] 配置 robots.txt

## 🚀 Phase 2: GitHub 與 Azure 設定（待完成）

- [ ] 在 GitHub 建立 repository: `smartsequence/DocEngine-Website`
- [ ] 推送程式碼到 GitHub
  ```bash
  git remote add origin https://github.com/smartsequence/DocEngine-Website.git
  git branch -M main
  git push -u origin main
  ```
- [ ] 在 Azure Portal 建立 Static Web App
  - 名稱: `docengine-website`
  - 計畫: Free
  - 區域: East Asia
  - 連接 GitHub repo
- [ ] 驗證首次自動部署成功
- [ ] 記錄預設 URL

## 🌐 Phase 3: 自訂域名配置（待完成）

- [ ] 在 Azure 新增自訂域名: `www.docengine.com`
- [ ] 在域名註冊商設定 CNAME 記錄
  ```
  類型: CNAME
  名稱: www
  值: [Azure 提供的值]
  ```
- [ ] 等待 DNS 傳播（5-60 分鐘）
- [ ] 驗證域名
- [ ] 等待 SSL 憑證配置（5-10 分鐘）
- [ ] 測試 HTTPS 訪問

## 🎨 Phase 4: 內容開發（待完成）

### 首頁 (index.astro)
- [ ] 設計 Hero Section
  - [ ] 主標題
  - [ ] 副標題
  - [ ] CTA 按鈕（開始免費試用）
  - [ ] 產品截圖/示意圖
- [ ] 功能特色概覽（3-4 個核心功能）
  - [ ] AI 智能分析
  - [ ] 多語言支援
  - [ ] Agent 架構
  - [ ] 即時同步
- [ ] 使用案例展示
- [ ] 客戶評價（如有）
- [ ] 定價簡介
- [ ] FAQ 快速解答
- [ ] 底部 CTA

### 功能特色頁 (features.astro)
- [ ] 建立頁面文件
- [ ] 詳細功能說明
  - [ ] 🤖 AI 智能分析
  - [ ] 📊 程式碼分析 (C#/VB.NET/ASPX)
  - [ ] 🗄️ 資料庫分析 (Oracle)
  - [ ] 📝 自動生成文件
  - [ ] 🔄 版本控制
  - [ ] 🌐 Agent 架構
  - [ ] 🚀 SignalR 即時通訊
- [ ] 每個功能配圖
- [ ] 技術細節說明

### 定價頁面 (pricing.astro)
- [ ] 建立頁面文件
- [ ] 設計定價卡片
- [ ] 定價方案
  - [ ] 免費試用（14 天）
  - [ ] 個人版
  - [ ] 團隊版
  - [ ] 企業版
- [ ] 功能比較表
- [ ] FAQ
- [ ] CTA 按鈕

### 聯絡頁面 (contact.astro)
- [ ] 建立頁面文件
- [ ] 聯絡表單
  - [ ] 姓名
  - [ ] Email
  - [ ] 公司（可選）
  - [ ] 訊息
  - [ ] 送出按鈕
- [ ] 聯絡資訊
  - [ ] Email: support@docengine.com
  - [ ] 社交媒體連結
- [ ] 整合表單 API（待定）

### 關於我們頁面 (about.astro)
- [ ] 更新現有頁面
- [ ] 公司簡介
- [ ] 團隊介紹
- [ ] 使命與願景
- [ ] 技術優勢

### Header & Footer
- [ ] 更新 Header.astro
  - [ ] Logo
  - [ ] 導航選單
  - [ ] CTA 按鈕
  - [ ] 行動版選單
- [ ] 更新 Footer.astro
  - [ ] 公司資訊
  - [ ] 快速連結
  - [ ] 社交媒體
  - [ ] 版權聲明

## 🖼️ Phase 5: 資源與素材（待完成）

- [ ] 設計或取得 Logo
- [ ] 準備產品截圖
- [ ] 準備功能示意圖
- [ ] 準備 favicon
- [ ] 準備 Open Graph 圖片
- [ ] 優化所有圖片（WebP 格式）

## 🔍 Phase 6: SEO 優化（待完成）

- [ ] 更新所有頁面的 meta 標籤
  - [ ] title
  - [ ] description
  - [ ] keywords
- [ ] 新增 Open Graph 標籤
  - [ ] og:title
  - [ ] og:description
  - [ ] og:image
  - [ ] og:url
- [ ] 新增 Twitter Card 標籤
- [ ] 建立 Schema.org 結構化資料
- [ ] 優化圖片 alt 文字
- [ ] 確保語意化 HTML
- [ ] 檢查 Core Web Vitals

## 📊 Phase 7: 分析與監控（待完成）

- [ ] 設定 Google Analytics 4
  - [ ] 建立 GA4 帳號
  - [ ] 取得追蹤 ID
  - [ ] 整合到 BaseHead.astro
- [ ] 設定 Google Search Console
  - [ ] 驗證網站所有權
  - [ ] 提交 sitemap
  - [ ] 設定 URL 檢查
- [ ] 設定 Azure Monitor
  - [ ] 啟用監控
  - [ ] 設定告警規則
- [ ] 效能監控
  - [ ] Lighthouse 測試
  - [ ] PageSpeed Insights 測試

## 🧪 Phase 8: 測試與優化（待完成）

- [ ] 功能測試
  - [ ] 所有連結正常
  - [ ] 表單提交正常
  - [ ] 圖片載入正常
- [ ] 瀏覽器相容性測試
  - [ ] Chrome
  - [ ] Firefox
  - [ ] Safari
  - [ ] Edge
- [ ] 行動裝置測試
  - [ ] iOS Safari
  - [ ] Android Chrome
  - [ ] 平板裝置
- [ ] 效能優化
  - [ ] 圖片優化
  - [ ] CSS 優化
  - [ ] JavaScript 優化
  - [ ] 快取策略
- [ ] 無障礙測試 (A11y)
  - [ ] 鍵盤導航
  - [ ] 螢幕閱讀器
  - [ ] 色彩對比

## 🚢 Phase 9: 上線準備（待完成）

- [ ] UAT 測試
- [ ] 內容校對
- [ ] 法律文件
  - [ ] 隱私政策
  - [ ] 服務條款
  - [ ] Cookie 政策
- [ ] 備份策略
- [ ] 監控告警設定
- [ ] 文檔完善
- [ ] 正式上線公告

## 🔮 Phase 10: 未來擴展（可選）

- [ ] 多語言支援 (i18n)
  - [ ] 英文版
  - [ ] 日文版（可選）
- [ ] 搜尋功能
- [ ] 線上聊天支援
- [ ] 客戶登入區
- [ ] 產品 Demo 影片
- [ ] 教學影片
- [ ] 整合 CRM
- [ ] Email 行銷整合
- [ ] 獨立文檔站 (docs.docengine.com)
- [ ] 部落格系統 (blog.docengine.com)
- [ ] 社群論壇
- [ ] 開發者平台

## 📝 注意事項

### 優先級
1. **P0 (最高)**: Phase 2-3（GitHub 與 Azure 設定）
2. **P1 (高)**: Phase 4（內容開發）
3. **P2 (中)**: Phase 5-6（資源與 SEO）
4. **P3 (低)**: Phase 7-9（分析、測試、上線）
5. **P4 (可選)**: Phase 10（未來擴展）

### 時程預估
- Phase 2-3: 1 天
- Phase 4: 1-2 週
- Phase 5-6: 3-5 天
- Phase 7-9: 1 週
- **總計**: 約 3-4 週

### 資源需求
- 設計師（Logo、UI 設計）
- 文案撰寫（中英文）
- 產品截圖/影片
- 域名（docengine.com）
- Azure 訂閱

---

**建立日期**: 2026-01-25  
**最後更新**: 2026-01-25  
**維護者**: DocEngine Team

**提示**: 完成項目後記得打勾 ✅
