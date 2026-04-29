---
name: ai-web-package-react
description: Convert AI-generated frontend prototypes from Gemini Canvas, Claude, v0, or other AI tools into a maintainable, buildable, and deployable React/Vite static website. Use this skill when the user wants to convert raw HTML, CSS, JavaScript, React, Vue, or mixed frontend code into a self-hostable React static web project for IIS or Zeabur.
---

# ai-web-package-react

## Skill 定位

本 Skill 的定位是：

將 AI 工具產出的前端原型，標準化轉換成可維護、可建置、可部署的 React/Vite 靜態網站專案。

這個 Skill 主要用於整理 Gemini Canvas、Claude、v0 或其他 AI 工具產出的前端程式碼，並將其轉換成可以自行部署的 React + Vite 靜態網站。

---

## 支援的來源格式

本 Skill 可處理以下來源：

- 原生 HTML / CSS / JavaScript
- 單一 `index.html`
- Gemini Canvas 匯出的前端頁面
- Claude 產生的 HTML / React / Vue 頁面
- v0 產生的 React 頁面
- 結構混亂但可整理的 React 專案
- Vue 靜態頁面，並轉換為 React
- 混合型靜態前端專案

> 繁體中文註解：這個 Skill 的重點不是重新設計 UI，而是把 AI 產出的原型整理成正式專案結構，讓它可以建置、部署與後續維護。

---

## 主要目標

Codex 使用本 Skill 時，應將使用者提供的前端專案轉換為：

- 可維護的 React 專案
- 可透過 Vite 建置
- 可部署為靜態網站
- 可依需求套用 IIS 或 Zeabur 部署設定

目前支援兩個部署 Profile：

- `ai-web-package-react-iis`
- `ai-web-package-react-zeabur`

---

## 預設技術棧

除非使用者另有指定，否則預設使用：

- React
- Vite
- JavaScript
- CSS / CSS Modules / structured global CSS
- Static Website
- Client-side rendering
- No backend
- No database
- No authentication

> 繁體中文註解：目前這個 Skill 以「靜態前端網站」為主，不主動加入後端、資料庫、登入驗證或 API 架構，避免把單純原型轉換成過度複雜的系統。

---

## 預設輸出結構

轉換後的專案建議使用以下結構：

```txt
project-root/
├─ public/
│  └─ assets/
│
├─ src/
│  ├─ assets/
│  ├─ components/
│  ├─ pages/
│  ├─ hooks/
│  ├─ utils/
│  ├─ styles/
│  ├─ App.jsx
│  └─ main.jsx
│
├─ index.html
├─ package.json
├─ vite.config.js
├─ README.md
└─ .gitignore
```

> 繁體中文註解：如果原始專案很小，不需要硬拆太多資料夾。小型專案可以保留簡潔結構，避免過度工程化。

---

## 工作流程

### 1. 初始檢查

在修改程式前，Codex 應先檢查：

- 原始專案是 HTML、React、Vue 或混合格式
- 主要進入點是哪個檔案
- CSS 放在哪裡
- JavaScript 放在哪裡
- 圖片與靜態資源放在哪裡
- 是否使用 CDN
- 是否有 inline CSS
- 是否有 inline JavaScript
- 是否有直接操作 DOM
- 是否有路由需求
- 是否有 API 呼叫
- 是否指定部署目標：IIS 或 Zeabur

> 繁體中文註解：這一步是為了避免直接硬改程式，導致原本 AI 工具產出的畫面或互動壞掉。

### 2. HTML 轉 React 規則

當來源是原生 HTML 時：

- 將 HTML 轉為 JSX
- 將 `class` 改成 `className`
- 將 `for` 改成 `htmlFor`
- 將 inline event，例如 `onclick`，改成 React event handler
- 將重複區塊整理成 components
- 將大量 inline CSS 搬到 `src/styles/`
- 將圖片與靜態資源搬到 `public/assets/` 或 `src/assets/`
- 保留原本版面與視覺風格
- 不主動重設計 UI
- 不主動改變原本的文案與內容

> 繁體中文註解：轉換時優先保留原本 AI 產出的畫面，除非使用者明確要求重新設計。

### 3. JavaScript 轉 React 規則

當來源是原生 JavaScript 時：

- 將 DOM 操作改成 React state
- 將互動事件改成 component function
- 將工具函式搬到 `src/utils/`
- 將頁面邏輯放在對應 component 或 page 中
- 移除未使用的 script
- 避免不必要的全域變數
- 避免繼續使用 jQuery，除非原專案高度依賴且短期難以移除
- 保留瀏覽器端可運作的 API，例如 localStorage、fetch、canvas 等

> 繁體中文註解：不是所有 DOM 操作都要硬改。若是短期轉換風險太高，可以先保留，但要在 README 註記技術債。

### 4. Vue 轉 React 規則

當來源是 Vue 時：

- 將 Vue component 轉為 React function component
- 將 `data()` 轉成 `useState`
- 將 `computed` 轉成 derived value 或 `useMemo`
- 將 `methods` 轉成 component 內部 function
- 將 `v-if` 轉成 React conditional rendering
- 將 `v-for` 轉成 `.map()`
- 將 `v-model` 轉成 controlled component
- 將 lifecycle hooks 轉成 `useEffect`
- 移除 Vue 相關依賴

> 繁體中文註解：Vue 轉 React 時，不只是語法替換，而是要轉換資料流與元件邏輯。

### 5. React 專案整理規則

當來源已經是 React 時：

- 檢查專案是否能建置
- 修正 import path
- 修正 asset path
- 移除未使用 import
- 整理 component 結構
- 確認 `main.jsx` 與 `App.jsx` 正常
- 確認 Vite 設定正確
- 確認 `npm install` 可執行
- 確認 `npm run build` 可成功

> 繁體中文註解：如果來源本來就是 React，不需要重新轉換整包程式，優先做清理、修正與標準化。

---

## 部署 Profile 選擇

### IIS Profile

當使用者提到以下關鍵字時，套用 `ai-web-package-react-iis`：

- IIS
- Windows Server
- web.config
- 部署到 IIS
- 靜態網站放 IIS
- Windows 主機上架

### Zeabur Profile

當使用者提到以下關鍵字時，套用 `ai-web-package-react-zeabur`：

- Zeabur
- zeabur.app
- 上架到 Zeabur
- Zeabur static
- Zeabur 部署
- GitHub + Zeabur

> 繁體中文註解：主 Skill 負責轉換與標準化，Profile 負責處理不同部署環境的差異。

---

## 品質檢查

完成轉換後，Codex 必須檢查：

- 專案是否有清楚的 React/Vite 結構
- 是否有 `package.json`
- 是否有 `vite.config.js`
- 是否有 `src/main.jsx`
- 是否有 `src/App.jsx`
- `npm install` 是否可執行
- `npm run build` 是否可成功
- 是否產生 `dist/`
- 靜態資源是否沒有 broken path
- README 是否說明本地開發與部署方式
- IIS 或 Zeabur 設定檔是否已加入
- 是否沒有加入不必要的後端邏輯
- 是否沒有提交 API key、token 或密碼

---

## README 必須包含

每次轉換後都要建立或更新 `README.md`，內容包含：

1. 專案說明
2. 技術棧
3. 原始來源說明
4. 本地開發方式
5. 建置方式
6. 部署方式
7. 資料夾結構
8. 已完成的轉換項目
9. 已知限制或待修正項目

---

## 預設驗證指令

```bash
npm install
npm run build
```

如果 build 失敗，Codex 應該先修正錯誤。

如果錯誤來自缺少圖片、影片或其他使用者未提供的靜態資源，應在 README 中註記。

---

## 禁止事項

Codex 不應該：

- 未經要求改成 Next.js
- 未經要求加入後端
- 未經要求加入資料庫
- 未經要求加入登入驗證
- 未經要求重新設計 UI
- 任意刪除使用者提供的素材
- 寫死本機絕對路徑，例如 `C:\Users\...`
- 提交 API key、token、密碼
- 引入不必要的大型 UI library
- 將簡單靜態頁面過度工程化

---

## 最終回覆格式

完成任務後，Codex 回覆需包含：

1. 轉換了什麼
2. 套用了哪個部署 Profile
3. 建立或修改了哪些檔案
4. 如何在本地執行
5. 如何建置
6. 如何部署
7. 有哪些假設或尚未解決的問題
