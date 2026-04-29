# ai-web-package-react-zeabur

## Profile 定位

本 Profile 用於將 React/Vite 靜態網站整理成可部署到 Zeabur 的格式。

適用場景：

- Zeabur static hosting
- GitHub + Zeabur 部署
- 快速展示 MVP
- AI 產出的前端 Demo 快速上線

---

## 使用時機

當使用者提到以下關鍵字時，套用本 Profile：

- Zeabur
- zeabur.app
- 上架到 Zeabur
- Zeabur static
- Zeabur 部署
- GitHub + Zeabur

> 繁體中文註解：Zeabur Profile 的重點是讓專案可以透過 GitHub Repo 自動 build 並部署。

---

## 必要輸出

專案需包含：

```txt
package.json
vite.config.js
README.md
zeabur.json
```

建置後需產生：

```txt
dist/
```

---

## package.json scripts

確認 `package.json` 至少包含：

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview --host 0.0.0.0"
  }
}
```

> 繁體中文註解：`preview --host 0.0.0.0` 是為了讓雲端環境或容器環境可以正確對外提供預覽服務。

---

## zeabur.json

建議加入：

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist"
}
```

> 繁體中文註解：這個檔案讓 Zeabur 明確知道要執行哪個 build command，以及靜態輸出目錄在哪裡。

---

## Vite 設定

Zeabur 靜態部署預設使用：

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/',
})
```

> 繁體中文註解：Zeabur 通常部署在根路徑，因此 `base: '/'` 即可。只有在使用子路徑部署時才需要改。

---

## Zeabur 部署流程

README 中需補上以下流程：

### 1. 本地測試

```bash
npm install
npm run dev
```

### 2. 建置

```bash
npm run build
```

### 3. 推送到 GitHub

```bash
git init
git add .
git commit -m "init react static site"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

### 4. Zeabur 部署

在 Zeabur 建立新專案：

```txt
Create Project
→ Deploy from GitHub
→ Select Repository
→ Set Build Command: npm run build
→ Set Output Directory: dist
→ Deploy
```

---

## Zeabur 驗證清單

部署後需確認：

- Zeabur build log 沒有錯誤
- 首頁可以正常開啟
- CSS 正常載入
- JavaScript 正常載入
- 圖片正常載入
- Browser Console 沒有 asset missing error
- 沒有依賴未設定的環境變數
- `dist/` 是實際輸出目錄

---

## Zeabur 常見問題

### 1. Build failed

可能原因：

- `package.json` scripts 錯誤
- 缺少 dependency
- 本地未先執行過 `npm run build`
- AI 工具產出的程式有語法錯誤

### 2. 部署成功但畫面空白

可能原因：

- Vite `base` 設定錯誤
- 靜態資源路徑錯誤
- React entry point 錯誤
- Browser Console 有 JavaScript error

### 3. 圖片沒有顯示

可能原因：

- 圖片沒有放進 `public/` 或 `src/assets/`
- import path 錯誤
- 原始 HTML 使用了不存在的相對路徑
