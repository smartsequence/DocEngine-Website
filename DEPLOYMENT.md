# DocEngine 官網部署指南

## 📋 前置準備

### 1. GitHub Repository

已建立 GitHub Repository:
- **Organization**: smartsequence
- **Repository**: DocEngine-Website
- **URL**: https://github.com/smartsequence/DocEngine-Website

### 2. Azure 帳號

需要有 Azure 訂閱帳號（可使用免費方案）

## 🚀 部署步驟

### Step 1: 推送程式碼到 GitHub

```bash
# 在本地專案目錄
cd C:\charleen\DocEngine-Website

# 添加遠端倉庫
git remote add origin https://github.com/smartsequence/DocEngine-Website.git

# 重命名分支為 main
git branch -M main

# 推送到 GitHub
git push -u origin main
```

### Step 2: 建立 Azure Static Web App

#### 方法 A: 透過 Azure Portal（推薦）

1. 登入 [Azure Portal](https://portal.azure.com)

2. 點擊「建立資源」→ 搜尋「Static Web Apps」

3. 填寫基本資訊：
   - **訂閱**: 選擇您的訂閱
   - **資源群組**: `DocEngine-Resources`（新建或使用現有）
   - **名稱**: `docengine-website`
   - **計畫類型**: `Free`
   - **區域**: `East Asia`

4. 部署詳細資料：
   - **來源**: `GitHub`
   - **組織**: `smartsequence`
   - **存放庫**: `DocEngine-Website`
   - **分支**: `main`

5. 建置詳細資料：
   - **建置預設**: `Astro`
   - **應用程式位置**: `/`
   - **API 位置**: （留空）
   - **輸出位置**: `dist`

6. 點擊「檢閱 + 建立」→「建立」

7. Azure 會自動：
   - 在您的 GitHub repo 建立 workflow 文件（或更新現有的）
   - 建立 GitHub Secret: `AZURE_STATIC_WEB_APPS_API_TOKEN`
   - 觸發第一次部署

#### 方法 B: 透過 Azure CLI

```bash
# 安裝 Azure CLI (如果尚未安裝)
# https://docs.microsoft.com/cli/azure/install-azure-cli

# 登入 Azure
az login

# 建立資源群組（如果不存在）
az group create --name DocEngine-Resources --location eastasia

# 建立 Static Web App
az staticwebapp create \
  --name docengine-website \
  --resource-group DocEngine-Resources \
  --source https://github.com/smartsequence/DocEngine-Website \
  --location eastasia \
  --branch main \
  --app-location "/" \
  --output-location "dist" \
  --login-with-github
```

### Step 3: 驗證部署

1. 部署完成後，Azure 會提供一個預設 URL：
   ```
   https://docengine-website-xxxxx.azurestaticapps.net
   ```

2. 在瀏覽器開啟該 URL，確認網站正常運行

3. 檢查 GitHub Actions：
   - 前往 https://github.com/smartsequence/DocEngine-Website/actions
   - 確認 workflow 執行成功（綠色勾勾）

### Step 4: 配置自訂域名

#### 4.1 在 Azure Portal 設定

1. 進入 Azure Portal → 您的 Static Web App
2. 左側選單 → 「設定」→「自訂域名」
3. 點擊「+ 新增」
4. 選擇「自訂域名 (CNAME)」
5. 輸入域名：`www.docengine.com`
6. 點擊「下一步」

#### 4.2 在域名註冊商設定 DNS

Azure 會提供 CNAME 記錄，例如：

```
類型: CNAME
名稱: www
值: docengine-website-xxxxx.azurestaticapps.net
TTL: 3600
```

在您的域名註冊商（如 GoDaddy、Namecheap、Cloudflare）設定此 CNAME 記錄。

#### 4.3 驗證域名

1. DNS 設定完成後（可能需要等待 5-60 分鐘）
2. 回到 Azure Portal，點擊「驗證」
3. 驗證成功後，Azure 會自動配置 SSL 憑證（Let's Encrypt）
4. 等待 5-10 分鐘，SSL 憑證配置完成

#### 4.4 測試自訂域名

在瀏覽器開啟：
```
https://www.docengine.com
```

確認：
- ✅ 網站正常顯示
- ✅ HTTPS 正常運作（綠色鎖頭）
- ✅ 無憑證錯誤

### Step 5: 配置根域名（可選）

如果您也想讓 `docengine.com`（不含 www）也能訪問：

1. 在 Azure Static Web App 新增另一個自訂域名：`docengine.com`
2. Azure 會提供 A 記錄的 IP 位址
3. 在域名註冊商設定 A 記錄：
   ```
   類型: A
   名稱: @
   值: [Azure 提供的 IP]
   TTL: 3600
   ```

## 🔄 持續部署

### 自動部署

每次推送到 `main` 分支時，GitHub Actions 會自動：

1. ✅ 檢出程式碼
2. ✅ 安裝 Node.js 20
3. ✅ 安裝依賴 (`npm ci`)
4. ✅ 建置專案 (`npm run build`)
5. ✅ 部署到 Azure Static Web Apps
6. ✅ 全球 CDN 更新

### Pull Request 預覽

每個 Pull Request 會自動建立預覽環境：
- 預覽 URL: `https://xxx-preview.azurestaticapps.net`
- PR 合併或關閉後自動清理

## 🔧 環境變數設定（如需要）

在 Azure Portal 設定環境變數：

1. 進入 Static Web App
2. 左側選單 → 「設定」→「環境變數」
3. 新增變數：
   ```
   ASTRO_SITE_URL=https://www.docengine.com
   ASTRO_APP_URL=https://app.docengine.com
   ASTRO_API_URL=https://api.docengine.com
   ```

## 📊 監控與分析

### Azure Monitor

1. 進入 Static Web App
2. 左側選單 → 「監視」→「計量」
3. 可查看：
   - 請求數
   - 資料傳輸量
   - 錯誤率
   - 回應時間

### Google Analytics（建議設定）

1. 建立 Google Analytics 4 帳號
2. 取得追蹤 ID
3. 在 `src/components/BaseHead.astro` 加入追蹤程式碼

### Google Search Console（建議設定）

1. 前往 [Google Search Console](https://search.google.com/search-console)
2. 新增網站：`https://www.docengine.com`
3. 驗證網站所有權
4. 提交 sitemap：`https://www.docengine.com/sitemap-index.xml`

## 🔒 安全性檢查清單

- ✅ HTTPS 已啟用
- ✅ 安全標頭已配置（在 `staticwebapp.config.json`）
- ✅ CSP (Content Security Policy) 已設定
- ✅ XSS 防護已啟用
- ✅ CSRF 防護已啟用
- ✅ DDoS 防護（Azure 內建）

## 🐛 故障排除

### 問題 1: 部署失敗

**解決方法**：
1. 檢查 GitHub Actions 日誌
2. 確認 `package.json` 的 scripts 正確
3. 確認 `astro.config.mjs` 配置正確
4. 本地測試 `npm run build` 是否成功

### 問題 2: 自訂域名無法訪問

**解決方法**：
1. 檢查 DNS 設定是否正確
2. 等待 DNS 傳播（最多 48 小時，通常 1 小時內）
3. 使用 `nslookup www.docengine.com` 檢查 DNS
4. 清除瀏覽器快取

### 問題 3: SSL 憑證錯誤

**解決方法**：
1. 確認域名驗證已完成
2. 等待 SSL 憑證配置（5-10 分鐘）
3. 如果超過 1 小時仍有問題，聯絡 Azure 支援

## 📞 支援資源

- [Azure Static Web Apps 文檔](https://docs.microsoft.com/azure/static-web-apps/)
- [Astro 文檔](https://docs.astro.build/)
- [GitHub Actions 文檔](https://docs.github.com/actions)

## 📝 檢查清單

部署前確認：

- [ ] 程式碼已推送到 GitHub
- [ ] Azure Static Web App 已建立
- [ ] GitHub Actions workflow 執行成功
- [ ] 預設 URL 可正常訪問
- [ ] 自訂域名已設定
- [ ] DNS 記錄已配置
- [ ] SSL 憑證已配置
- [ ] Google Analytics 已設定（可選）
- [ ] Google Search Console 已設定（可選）
- [ ] 監控已啟用

---

**建立日期**: 2026-01-25  
**最後更新**: 2026-01-25  
**維護者**: DocEngine Team
