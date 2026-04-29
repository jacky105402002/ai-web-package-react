# 高雄旅遊網

這個專案由 Gemini Canvas 複製出的 React 頁面整理而成，已轉換為可建置、可部署的 React + Vite 靜態網站。

## 技術棧

- React
- Vite
- Tailwind CSS
- lucide-react
- Static Website

## 原始來源

來源為使用者提供的 Gemini Canvas React 程式碼。轉換時保留原始 UI、文案、mock data、AI 客服視窗與 AI 行程規劃互動。

## 本地開發

```bash
npm install
npm run dev
```

## 建置

```bash
npm run build
```

建置完成後會產生：

```txt
dist/
```

## IIS 部署

1. 執行 `npm run build`
2. 將 `dist/` 內所有檔案複製到 IIS Site 對應目錄
3. 確認 `dist/web.config` 一起被複製
4. 確認 IIS 已安裝 URL Rewrite
5. 開啟網站測試首頁、CSS、JS、圖片與重新整理頁面

若部署在 IIS 子目錄，例如 `https://example.com/kaohsiung/`，請將 `vite.config.js` 的 `base` 改成：

```js
base: '/kaohsiung/',
```

再重新執行 `npm run build`。

## Gemini AI 設定

AI 客服與 AI 行程規劃會讀取：

```txt
VITE_GEMINI_API_KEY
```

請參考 `.env.example` 建立 `.env`。如果沒有設定 API Key，網站仍可建置與部署，AI 功能會顯示尚未設定的提示訊息。

## 資料夾結構

```txt
project-root/
├─ public/
│  └─ web.config
├─ src/
│  ├─ styles/
│  │  └─ index.css
│  ├─ App.jsx
│  └─ main.jsx
├─ index.html
├─ package.json
├─ postcss.config.js
├─ tailwind.config.js
├─ vite.config.js
└─ README.md
```

## 已完成轉換項目

- 建立 React + Vite 專案結構
- 保留 Gemini Canvas 原始頁面與互動邏輯
- 將 inline CSS 動畫整理到 `src/styles/index.css`
- 將 Gemini API Key 改為使用環境變數
- 加入 IIS `web.config`
- 驗證可執行 `npm run build`

## 已知限制

- 目前圖片使用 `picsum.photos` 遠端示意圖，IIS 主機需可連線到外部圖片服務才會正常顯示。
- 頁面中的內部連結是示範路徑，若沒有實際路由或頁面，點擊後會由 SPA fallback 回到首頁。
- 前端直接呼叫 Gemini API 只適合 demo。正式站若要保護 API Key，建議改由後端代理。
