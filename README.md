# Notablog - 從 Notion 生成靜態部落格

> 一個強大的靜態網站生成器，將 Notion 資料庫轉換為精美的部落格網站

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## ✨ 特色功能

- 🚀 **從 Notion 直接生成**：使用 Notion 作為 CMS，無需複雜的後台管理
- 🎨 **可自訂主題**：使用 EJS 模板引擎，輕鬆客製化網站外觀
- ⚡ **快速生成**：智能快取機制，只更新變更的頁面
- 🔗 **內部連結轉換**：自動處理 Notion 內部連結
- 🖼️ **圖片本地化**：自動下載 Notion 圖片到本地
- 🌐 **外部連結支援**：可在 Notion 中設定外部連結，自動跳過渲染
- 📱 **響應式設計**：內建的 pure-ejs 主題支援各種裝置

## 📦 安裝

### 前置需求

- Node.js >= 15
- npm 或 yarn

### 從原始碼安裝

```bash
# 克隆專案
git clone https://github.com/YOUR_USERNAME/notablog.git
cd notablog

# 安裝依賴
npm install --legacy-peer-deps

# 編譯專案
npm run build:module
```

## 🚀 快速開始

### 1. 準備 Notion 資料庫

在 Notion 中建立一個資料庫，包含以下欄位：

| 欄位名稱    | 類型   | 說明                           |
| ----------- | ------ | ------------------------------ |
| title       | 標題   | 文章標題（自動）               |
| tags        | 多選   | 文章標籤                       |
| publish     | 勾選框 | 是否發布                       |
| inMenu      | 勾選框 | 是否顯示在導航欄               |
| inList      | 勾選框 | 是否顯示在首頁列表             |
| template    | 單選   | 使用的模板（通常選 `post`）    |
| url         | 文字   | 自訂網址（選填，支援外部連結） |
| description | 文字   | 文章描述                       |
| date        | 日期   | 發布日期                       |

### 2. 建立專案目錄

```bash
# 使用 notablog-starter 作為範本
mkdir my-blog
cd my-blog
```

在專案目錄中建立 `config.json`：

```json
{
  "url": "你的 Notion 資料庫網址",
  "theme": "pure-ejs",
  "autoSlug": true
}
```

### 3. 生成並預覽網站

```bash
# 從專案根目錄執行生成命令
node bin/cli.js generate my-blog

# 啟動本地預覽伺服器
node bin/cli.js preview my-blog
```

**在瀏覽器中查看**：

1. 執行 `preview` 命令後，終端機會顯示伺服器已啟動
2. 打開瀏覽器，訪問 `http://localhost:3000`
3. 即可看到您的部落格網站

**停止預覽伺服器**：

- 在終端機中按 `Ctrl + C`

**提示**：

- 修改 Notion 內容後，需要重新執行 `generate` 命令
- 然後在瀏覽器中按 `Cmd + Shift + R`（Mac）或 `Ctrl + Shift + R`（Windows）強制刷新頁面

## 📖 使用說明

### 命令列工具

#### `generate` - 生成靜態網站

```bash
node bin/cli.js generate <path_to_blog> [options]

選項：
  -v, --verbose    顯示詳細日誌
  --fresh          清除快取重新生成
```

#### `preview` - 本地預覽

```bash
node bin/cli.js preview <path_to_blog> [options]

選項：
  -v, --verbose    顯示詳細日誌
```

### 外部連結功能

在 Notion 的 `url` 欄位中填入完整的 URL（以 `http://` 或 `https://` 開頭），系統會自動識別並跳過渲染，直接使用該連結。

**範例**：

- `url: https://example.com` → 直接連結到外部網站
- `url: my-post` → 生成 `my-post.html`
- `url: ` (留空) → 自動生成網址

## 🛠️ 開發指南

### 專案結構

```
notablog/
├── bin/                  # CLI 執行檔
│   └── cli.js
├── dist/                 # 編譯後的程式碼
│   ├── index.js
│   └── index.esm.js
├── src/                  # 源碼
│   ├── commands/         # CLI 命令
│   ├── utils/            # 工具函數
│   ├── cache.ts          # 快取管理
│   ├── config.ts         # 配置管理
│   ├── parseTable.ts     # Notion 表格解析
│   ├── renderPost.ts     # 文章渲染
│   ├── renderer.ts       # 模板渲染引擎
│   └── templateProvider.ts # 模板提供者
├── notablog-starter/     # 範例部落格
└── package.json
```

### 修改源碼

```bash
# 開發模式（監聽檔案變更）
npm run dev

# 編譯
npm run build:module

# 執行測試
npm test
```

### 自訂主題

主題檔案位於 `<your-blog>/themes/<theme-name>/`：

```
themes/pure-ejs/
├── manifest.json         # 主題配置
├── assets/              # 靜態資源
│   └── css/
│       └── theme.css
└── layouts/             # 模板檔案
    ├── index.html       # 首頁
    ├── post.html        # 文章頁
    ├── tag.html         # 標籤頁
    └── partials/        # 部分組件
        ├── head.html
        ├── navbar.html
        └── footer.html
```

## 🔧 技術細節

### 核心技術

- **TypeScript** - 型別安全的開發體驗
- **EJS / Squirrelly** - 靈活的模板引擎
- **Notion API** - 透過 notionapi-agent 存取 Notion 資料
- **NAST** - Notion Abstract Syntax Tree，用於處理 Notion 內容

### 主要修改

本專案基於 [dragonman225/notablog](https://github.com/dragonman225/notablog) 並進行了以下改進：

1. **外部連結支援**：在 `parseTable.ts` 和 `renderPost.ts` 中新增外部連結檢測
2. **TypeScript 配置優化**：修正 `tsconfig.json` 的 moduleResolution 設定
3. **渲染引擎修復**：修正 Squirrelly 渲染器的參數問題
4. **專案結構整理**：移除未使用的檔案和重複配置

## 📤 部署

### GitHub Pages

1. 生成網站：

```bash
node bin/cli.js generate notablog-starter
```

2. 部署 `notablog-starter/public/` 目錄到 GitHub Pages

### 其他平台

生成的靜態網站位於 `public/` 目錄，可以部署到：

- Netlify
- Vercel
- Cloudflare Pages
- 任何靜態網站託管服務

## 🤝 貢獻

歡迎提交 Issue 和 Pull Request！

## 📄 授權

MIT License - 詳見 [LICENSE](LICENSE) 檔案

## 🙏 致謝

- 原始專案：[dragonman225/notablog](https://github.com/dragonman225/notablog)
- Notion API：[notionapi-agent](https://github.com/dragonman225/notionapi-agent)

---

## 📚 詳細文檔

更多使用說明請參考 `notablog-starter/README.md`
