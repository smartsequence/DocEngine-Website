# Vercel + PostgreSQL 部署方案

**建立日期**: 2026-01-25  
**版本**: v1.0  
**狀態**: ✅ 完整解決方案

---

## ❓ 您的問題

### 用 PostgreSQL 還是可以部署在 Vercel 嗎？

**答案：可以！而且非常簡單。** ✅

---

## 🎯 Vercel + 資料庫的完整方案

### 核心概念

```
Vercel 平台架構：

┌─────────────────────────────────────────────────────┐
│              Vercel 部署平台                          │
├─────────────────────────────────────────────────────┤
│                                                       │
│  前端（靜態/SSR）                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ • Astro 網站                                │    │
│  │ • 靜態頁面 + Serverless Functions           │    │
│  │ • 全球 Edge Network CDN                     │    │
│  └─────────────────────────────────────────────┘    │
│                      ↕                               │
│  Serverless Functions（API）                         │
│  ┌─────────────────────────────────────────────┐    │
│  │ • 翻譯 API                                   │    │
│  │ • AI 客服 API                                │    │
│  │ • 資料庫查詢                                 │    │
│  └─────────────────────────────────────────────┘    │
│                      ↕                               │
└──────────────────────┼──────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────┐
│           外部資料庫服務（整合方案）                   │
├─────────────────────────────────────────────────────┤
│                                                       │
│  選項 1: Vercel Postgres ⭐ 推薦                      │
│  ┌─────────────────────────────────────────────┐    │
│  │ • 基於 Neon（Serverless PostgreSQL）        │    │
│  │ • Vercel 原生整合                            │    │
│  │ • 免費額度：0.5 GB 儲存                      │    │
│  │ • 自動擴展                                   │    │
│  │ • 一鍵設定                                   │    │
│  └─────────────────────────────────────────────┘    │
│                                                       │
│  選項 2: Neon（直接）                                │
│  ┌─────────────────────────────────────────────┐    │
│  │ • Serverless PostgreSQL                     │    │
│  │ • 免費額度：0.5 GB                           │    │
│  │ • 分支功能（Git-like）                       │    │
│  │ • 自動休眠（節省成本）                       │    │
│  └─────────────────────────────────────────────┘    │
│                                                       │
│  選項 3: Supabase                                    │
│  ┌─────────────────────────────────────────────┐    │
│  │ • PostgreSQL + 即時訂閱                      │    │
│  │ • 免費額度：500 MB                           │    │
│  │ • 內建認證和儲存                             │    │
│  │ • 完整後端服務                               │    │
│  └─────────────────────────────────────────────┘    │
│                                                       │
│  選項 4: Railway                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ • PostgreSQL + Redis + 其他服務              │    │
│  │ • 免費額度：$5 credit/月                     │    │
│  │ • 簡單易用                                   │    │
│  └─────────────────────────────────────────────┘    │
│                                                       │
└─────────────────────────────────────────────────────┘
```

---

## ⭐ 推薦方案：Vercel Postgres

### 為什麼推薦？

```
✅ Vercel 原生整合（一鍵設定）
✅ Serverless（按需付費）
✅ 免費額度充足（起步階段）
✅ 自動擴展（無需管理）
✅ 與 Vercel Functions 零延遲連接
```

### 快速設定（3 分鐘）

#### 1. 在 Vercel Dashboard 建立資料庫

```bash
# 方式 1：通過 Vercel Dashboard（推薦）
1. 進入專案設定
2. 點擊 "Storage" 標籤
3. 選擇 "Create Database"
4. 選擇 "Postgres"
5. 點擊 "Continue" → 完成！

# 方式 2：使用 Vercel CLI
vercel link
vercel postgres create
```

#### 2. Vercel 自動設定環境變數

```bash
# Vercel 自動添加這些環境變數：
POSTGRES_URL="postgres://..."
POSTGRES_PRISMA_URL="postgres://..."
POSTGRES_URL_NON_POOLING="postgres://..."
POSTGRES_USER="..."
POSTGRES_HOST="..."
POSTGRES_PASSWORD="..."
POSTGRES_DATABASE="..."

# 您不需要手動設定，Vercel 全自動！✅
```

#### 3. 在程式碼中使用

```typescript
// src/lib/db.ts
import { sql } from '@vercel/postgres';

export async function getContent(key: string, lang: string) {
  const { rows } = await sql`
    SELECT content_${lang} as content
    FROM contents
    WHERE key = ${key}
  `;
  
  return rows[0]?.content;
}

// 或使用 ORM（Prisma）
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export async function getContent(key: string, lang: string) {
  return await prisma.contents.findUnique({
    where: { key },
    select: { [`content_${lang}`]: true },
  });
}
```

---

## 💰 成本比較

### Vercel Postgres（基於 Neon）

| 方案 | 儲存空間 | 計算時數 | 連接數 | 價格 |
|------|---------|---------|--------|------|
| **Hobby** | 0.5 GB | 100 hours | 無限 | **$0** ✅ |
| **Pro** | 10 GB | 300 hours | 無限 | **$20/月** |

**免費額度足夠嗎？**

```
假設：
- 5 個主要頁面
- 每頁 50 個內容項目 = 250 項
- 每項平均 500 字元 = 125 KB
- 加上翻譯快取（10 種語言）= 1.25 MB

翻譯快取（假設 1000 次翻譯）：
- 每次翻譯 500 字元 = 500 KB
- 1000 次 = 500 MB

總計：約 50-100 MB ✅ 
遠低於 500 MB 免費額度！
```

### 其他方案比較

| 服務 | 免費額度 | 優點 | 缺點 |
|------|---------|------|------|
| **Vercel Postgres** | 0.5 GB | Vercel 整合最好 | 基於 Neon |
| **Neon** | 0.5 GB | 分支功能、自動休眠 | 需額外設定 |
| **Supabase** | 0.5 GB | 功能最多（認證/儲存） | 較複雜 |
| **Railway** | $5 credit | 多種服務 | Credit 用完要付費 |
| **PlanetScale** | 5 GB | MySQL、分支功能 | 不是 PostgreSQL |

---

## 🔧 完整實作範例

### 1. 安裝依賴

```bash
# 使用 Vercel Postgres SDK
npm install @vercel/postgres

# 或使用 Prisma ORM（推薦）
npm install @prisma/client
npm install -D prisma
```

### 2. 設定 Prisma Schema

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("POSTGRES_PRISMA_URL")
}

model Content {
  id          String   @id @default(uuid())
  key         String   @unique
  category    String
  contentZhTw String   @map("content_zh_tw")
  contentEn   String?  @map("content_en")
  createdAt   DateTime @default(now()) @map("created_at")
  updatedAt   DateTime @updatedAt @map("updated_at")

  translations TranslationCache[]
  embeddings   ContentEmbedding[]

  @@map("contents")
}

model TranslationCache {
  id              String   @id @default(uuid())
  contentId       String   @map("content_id")
  languageCode    String   @map("language_code")
  translatedText  String   @map("translated_text")
  createdAt       DateTime @default(now()) @map("created_at")
  expiresAt       DateTime @map("expires_at")

  content Content @relation(fields: [contentId], references: [id])

  @@unique([contentId, languageCode])
  @@map("translation_cache")
}

model ContentEmbedding {
  id           String   @id @default(uuid())
  contentId    String   @map("content_id")
  languageCode String   @map("language_code")
  embedding    Float[]  // PostgreSQL array
  createdAt    DateTime @default(now()) @map("created_at")

  content Content @relation(fields: [contentId], references: [id])

  @@unique([contentId, languageCode])
  @@map("content_embeddings")
}
```

### 3. 建立資料庫表格

```bash
# 初始化 Prisma
npx prisma init

# 生成遷移
npx prisma migrate dev --name init

# 生成 Prisma Client
npx prisma generate
```

### 4. API Route 範例

```typescript
// src/pages/api/content/[key].ts
import { PrismaClient } from '@prisma/client';
import type { APIRoute } from 'astro';

const prisma = new PrismaClient();

export const GET: APIRoute = async ({ params, url }) => {
  const { key } = params;
  const lang = url.searchParams.get('lang') || 'zh-TW';

  try {
    // 1. 從資料庫取得內容
    const content = await prisma.content.findUnique({
      where: { key },
    });

    if (!content) {
      return new Response(JSON.stringify({ error: 'Content not found' }), {
        status: 404,
      });
    }

    // 2. 取得對應語言的內容
    let text: string;
    
    if (lang === 'zh-TW') {
      text = content.contentZhTw;
    } else if (lang === 'en') {
      text = content.contentEn || content.contentZhTw;
    } else {
      // 3. 檢查翻譯快取
      const cached = await prisma.translationCache.findUnique({
        where: {
          contentId_languageCode: {
            contentId: content.id,
            languageCode: lang,
          },
        },
      });

      if (cached && cached.expiresAt > new Date()) {
        text = cached.translatedText;
      } else {
        // 4. 呼叫翻譯 API
        text = await translateText(content.contentZhTw, lang);
        
        // 5. 儲存快取
        await prisma.translationCache.upsert({
          where: {
            contentId_languageCode: {
              contentId: content.id,
              languageCode: lang,
            },
          },
          update: {
            translatedText: text,
            expiresAt: new Date(Date.now() + 30 * 24 * 60 * 60 * 1000),
          },
          create: {
            contentId: content.id,
            languageCode: lang,
            translatedText: text,
            expiresAt: new Date(Date.now() + 30 * 24 * 60 * 60 * 1000),
          },
        });
      }
    }

    return new Response(JSON.stringify({ content: text }), {
      status: 200,
      headers: {
        'Content-Type': 'application/json',
        'Cache-Control': 'public, s-maxage=3600, stale-while-revalidate=86400',
      },
    });
  } catch (error) {
    console.error('Error:', error);
    return new Response(JSON.stringify({ error: 'Internal server error' }), {
      status: 500,
    });
  }
};

async function translateText(text: string, lang: string): Promise<string> {
  // 使用 Google Translate API
  // 實作細節見前面的文件
  return text; // placeholder
}
```

---

## 🚀 部署流程

### 完整步驟

```bash
# 1. 確保程式碼已推送到 GitHub
git add -A
git commit -m "Add database integration"
git push

# 2. 在 Vercel 建立專案（如果還沒有）
vercel link

# 3. 建立 Postgres 資料庫
# 在 Vercel Dashboard → Storage → Create Database → Postgres

# 4. 執行資料庫遷移
vercel env pull .env.local  # 下載環境變數到本地
npx prisma migrate deploy   # 在生產環境執行遷移

# 5. 部署
git push  # Vercel 自動部署
```

---

## 📊 架構圖（更新）

```
┌───────────────────────────────────────────────────────┐
│                  使用者訪問                            │
└───────────────────┬───────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────────────────┐
│               Vercel Edge Network                      │
│               （全球 CDN）                             │
└───────────────────┬───────────────────────────────────┘
                    ↓
        ┌───────────┴──────────┐
        ↓                      ↓
┌──────────────┐      ┌──────────────────┐
│  靜態頁面     │      │ Serverless       │
│  (繁中/英文)  │      │ Functions        │
│              │      │ (其他語言 API)    │
│  SSG         │      │                  │
│  Astro       │      │  • 翻譯 API      │
│              │      │  • 資料查詢      │
│  SEO 完美    │      │  • AI 客服       │
└──────────────┘      └────────┬─────────┘
                               ↓
                    ┌──────────────────────┐
                    │  Vercel Postgres     │
                    │  (Neon)              │
                    │                      │
                    │  • contents          │
                    │  • translation_cache │
                    │  • embeddings        │
                    └──────────────────────┘
```

---

## ⚡ 效能優化

### 連接池設定

```typescript
// src/lib/db.ts
import { PrismaClient } from '@prisma/client';

// Vercel Serverless 環境優化
let prisma: PrismaClient;

if (process.env.NODE_ENV === 'production') {
  prisma = new PrismaClient({
    datasources: {
      db: {
        url: process.env.POSTGRES_PRISMA_URL, // 使用連接池 URL
      },
    },
  });
} else {
  // 開發環境
  if (!global.prisma) {
    global.prisma = new PrismaClient();
  }
  prisma = global.prisma;
}

export default prisma;
```

### 快取策略

```typescript
// 1. Prisma 查詢結果快取（記憶體）
const cache = new Map();

export async function getCachedContent(key: string) {
  if (cache.has(key)) {
    return cache.get(key);
  }
  
  const content = await prisma.content.findUnique({ where: { key } });
  cache.set(key, content);
  
  return content;
}

// 2. Vercel Edge Cache（HTTP 標頭）
return new Response(JSON.stringify(data), {
  headers: {
    'Cache-Control': 'public, s-maxage=3600, stale-while-revalidate=86400',
  },
});

// 3. 資料庫層快取（translation_cache 表）
// 已在上面實作
```

---

## 🔐 安全性

### 環境變數管理

```bash
# .env.local（本地開發，不要提交到 Git）
POSTGRES_URL="..."
POSTGRES_PRISMA_URL="..."
OPENAI_API_KEY="..."
GOOGLE_TRANSLATE_API_KEY="..."

# Vercel Dashboard（生產環境）
# Storage → Postgres → 自動設定資料庫環境變數
# Settings → Environment Variables → 手動添加 API Keys
```

### SQL 注入防護

```typescript
// ✅ 使用 Prisma（自動防護）
await prisma.content.findUnique({
  where: { key }, // Prisma 自動處理
});

// ✅ 使用參數化查詢
await sql`SELECT * FROM contents WHERE key = ${key}`;

// ❌ 不要字串拼接
await sql`SELECT * FROM contents WHERE key = '${key}'`; // 危險！
```

---

## 💡 最佳實踐

### 1. 使用 Prisma ORM（推薦）

**優點：**
- ✅ 類型安全（TypeScript）
- ✅ 自動遷移管理
- ✅ 查詢建構器
- ✅ 防止 SQL 注入

### 2. 分離關注點

```
src/
├─ lib/
│  ├─ db.ts              # 資料庫連接
│  ├─ prisma.ts          # Prisma Client
│  └─ content.ts         # 內容查詢邏輯
│
├─ pages/
│  └─ api/
│     └─ content/
│        └─ [key].ts     # API Route
```

### 3. 錯誤處理

```typescript
try {
  const content = await prisma.content.findUnique({ where: { key } });
} catch (error) {
  if (error.code === 'P2025') {
    // 記錄不存在
    return { error: 'Not found' };
  }
  // 其他錯誤
  console.error('Database error:', error);
  return { error: 'Internal error' };
}
```

---

## 📋 檢查清單

### 設定完成確認

- [ ] Vercel 專案已建立
- [ ] Postgres 資料庫已建立（Vercel Storage）
- [ ] 環境變數自動設定完成
- [ ] Prisma 已設定
- [ ] 資料庫遷移已執行
- [ ] API Routes 可以連接資料庫
- [ ] 本地開發環境正常
- [ ] 生產環境部署成功

---

## 🎯 總結

### ✅ Vercel + PostgreSQL 完全可行！

**方案：**
```
前端：Vercel（靜態 + Serverless Functions）
資料庫：Vercel Postgres（基於 Neon）
成本：$0 起步，自動擴展
```

**優點：**
- ✅ 一鍵整合
- ✅ 零配置（環境變數自動）
- ✅ 免費額度充足
- ✅ 自動擴展
- ✅ 低延遲連接

**您不需要：**
- ❌ 自己架設資料庫伺服器
- ❌ 手動設定連接字串
- ❌ 管理備份
- ❌ 擔心擴展性

**Vercel 全部幫您搞定！** 🎉

---

**建立日期**: 2026-01-25  
**維護者**: 智序資訊工作室  
**狀態**: ✅ Vercel + PostgreSQL 完整方案
