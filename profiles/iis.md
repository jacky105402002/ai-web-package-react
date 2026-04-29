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

如果部署時只複製 `dist/` 到 IIS，則 `web.config` 也應放進 `dist/`。

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
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/',
})
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

## IIS SPA Fallback

如果 React 專案使用 React Router 或其他前端路由，需加入：

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

- 首頁可以正常開啟
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
