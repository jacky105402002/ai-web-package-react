# ai-web-package-react-iis

## Profile 定位

本 Profile 用於將 React/Vite 靜態網站整理成可部署到 IIS 的格式。

適用場景：

- Windows Server
- IIS 靜態網站
- 企業內部 Windows 主機
- 政府標案或內網系統
- 需要使用 `web.config` 的部署環境

---

## 使用時機

當使用者提到以下關鍵字時，套用本 Profile：

- IIS
- Windows Server
- web.config
- 部署到 IIS
- 靜態網站放 IIS
- Windows 主機上架

> 繁體中文註解：這個 Profile 只處理 IIS 靜態部署，不處理 ASP.NET、PHP、Laravel 或後端服務。

---

## 必要輸出

專案需包含：

```txt
package.json
vite.config.js
README.md
web.config
```

建置後需產生：

```txt
dist/
```

如果部署時只複製 `dist/` 到 IIS，則 `web.config` 也應放進 `dist/`。可將 `web.config` 放在 `public/web.config`，讓 Vite build 自動複製到 `dist/web.config`。

---

## Vite 設定

### 重要備註：IIS 預設使用相對資源路徑

請讀取這個 repo 作為 skill 文件，使用 `ai-web-package-react`，並套用 `ai-web-package-react-iis` profile。IIS 部署請優先使用 `base: './'`，避免分站、子應用程式或虛擬目錄部署時 `/assets` 404。

除非使用者明確指定網站一定部署在固定根目錄，否則 IIS profile 預設使用：

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: './',
})
```

這會讓建置後的 `index.html` 使用：

```txt
./assets/index-xxxxx.js
./assets/index-xxxxx.css
```

而不是：

```txt
/assets/index-xxxxx.js
/assets/index-xxxxx.css
```

> 繁體中文註解：IIS 常見部署方式是建立分站、Application 或 Virtual Directory。若使用 `base: '/'`，CSS、JS、圖片會從 IIS 網站根目錄 `/assets/` 載入，分站部署時很容易 404。因此本 Profile 預設採用 `base: './'`。

如果使用者明確指定部署在網站根目錄，且確定所有資源都要從 domain root 載入，才可使用：

```js
base: '/',
```

如果使用者明確指定固定子路徑，例如：

```txt
https://example.com/my-app/
```

也可以使用：

```js
base: '/my-app/',
```

但在未確認部署路徑前，請不要主動使用固定絕對路徑。

---

## IIS 首頁入口正規化

IIS 的預設文件通常是 `index.html`、`default.htm` 或 `default.aspx`。因此套用 IIS profile 時，必須確認正式部署目錄的首頁入口是 `dist/index.html`。

Codex 應在初始檢查時判斷原始專案真正入口，例如：

- `index.html`
- `game.html`
- `home.html`
- `main.html`
- 使用者指定的其他 HTML 檔

處理規則：

- 如果來源入口已經是 `index.html`，保持 `dist/index.html` 為正式首頁。
- 如果來源入口不是 `index.html`，例如 `game.html`，建置後必須讓 `dist/index.html` 指向或等同該入口內容。
- 對高度依賴原生 DOM、inline CSS、inline JavaScript 的大型 AI 原型，可先將原始入口保留到 `public/{entry}.html`，再用 build 後處理把 `dist/{entry}.html` 複製成 `dist/index.html`。
- 若使用 React/Vite 外殼承載原始頁面，仍要確認 IIS 正式首頁不會因 iframe、路徑或 default document 設定而空白。

建議 postbuild 腳本範例：

```js
const fs = require('node:fs')
const path = require('node:path')

const distDir = path.resolve(__dirname, '..', 'dist')
const entryHtml = path.join(distDir, 'game.html')
const indexHtml = path.join(distDir, 'index.html')

if (!fs.existsSync(entryHtml)) {
  throw new Error(`Missing ${entryHtml}. Run vite build before this script.`)
}

fs.copyFileSync(entryHtml, indexHtml)
console.log('Copied dist/game.html to dist/index.html for IIS default document.')
```

`package.json` 可搭配：

```json
{
  "scripts": {
    "build": "vite build && node scripts/use-entry-as-index.cjs"
  }
}
```

> 繁體中文註解：不要假設原始 AI 原型的入口一定叫 `index.html`。只要正式站網址是資料夾路徑，例如 `https://example.com/my-app/`，IIS 通常會先找 `index.html`。如果真正入口是 `game.html`，就應在 build 流程中自動正規化，避免部署後首頁空白或出現錯誤。

---

## IIS SPA Fallback

如果 React 專案使用 React Router 或其他前端路由，需要加入：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="React SPA Fallback" stopProcessing="true">
          <match url=".*" />
          <conditions logicalGrouping="MatchAll">
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
          </conditions>
          <action type="Rewrite" url="index.html" />
        </rule>
      </rules>
    </rewrite>

    <staticContent>
      <remove fileExtension=".json" />
      <mimeMap fileExtension=".json" mimeType="application/json" />
      <remove fileExtension=".webmanifest" />
      <mimeMap fileExtension=".webmanifest" mimeType="application/manifest+json" />
      <remove fileExtension=".svg" />
      <mimeMap fileExtension=".svg" mimeType="image/svg+xml" />
    </staticContent>
  </system.webServer>
</configuration>
```

> 繁體中文註解：沒有前端路由時，這個設定不一定需要；但保留通常也不會影響一般靜態頁面。

---

## IIS 部署流程

README 中需補上以下流程：

```bash
npm install
npm run build
```

接著將 `dist/` 內容複製到 IIS 網站目錄，例如：

```txt
C:\inetpub\wwwroot\my-app
```

或使用者指定的路徑。

> 繁體中文註解：不要在 Skill 裡寫死實際 IIS 路徑，除非使用者有提供專案部署位置。

---

## IIS 驗證清單

部署後需確認：

- 首頁資料夾網址可以正常開啟，例如 `/my-app/`
- `dist/index.html` 是正式首頁入口
- 如果原始入口不是 `index.html`，已完成入口正規化
- CSS 正常載入
- JavaScript 正常載入
- 圖片正常載入
- 重新整理頁面不會 404
- 巢狀路由可以正常開啟
- Browser Console 沒有 asset missing error
- 沒有 MIME type error
- `web.config` 已放在正確位置

---

## IIS 常見問題

### 1. 重新整理後 404

可能原因：

- 沒有 `web.config`
- IIS 沒有啟用 URL Rewrite
- SPA fallback 沒有設定

### 2. CSS / JS 載入失敗

可能原因：

- `vite.config.js` 的 `base` 設定錯誤
- 部署到子目錄但仍使用 `base: '/'`

### 3. JSON 或 SVG 無法載入

可能原因：

- IIS MIME type 未設定
- `web.config` 沒有補上對應 mimeMap

### 4. 直接開 `/my-app/` 空白或顯示外殼錯誤

可能原因：

- 真正入口是 `game.html`、`home.html` 等非 `index.html` 檔案
- `dist/index.html` 不是正式首頁內容
- React 外殼 iframe 指向錯誤路徑
- IIS Default Document 沒有包含實際入口檔

處理方式：

- 優先讓 build 後的 `dist/index.html` 等同正式入口內容
- 若保留原入口檔，仍可同時輸出 `dist/game.html`，但資料夾網址必須能由 `dist/index.html` 正常啟動
