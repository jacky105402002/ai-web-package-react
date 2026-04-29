# Conversion Checklist

## 1. Source Inspection

- [ ] 確認來源格式：HTML / React / Vue / mixed static package
- [ ] 找到主要入口檔案
- [ ] 找到 CSS 檔案
- [ ] 找到 JavaScript 檔案
- [ ] 找到圖片與靜態資源
- [ ] 確認是否使用 CDN
- [ ] 確認是否有 inline CSS
- [ ] 確認是否有 inline JavaScript
- [ ] 確認是否有 DOM 操作
- [ ] 確認是否有路由
- [ ] 確認是否有 API 呼叫
- [ ] 確認部署目標：IIS / Zeabur / generic static

---

## 2. React Conversion

- [ ] 將 HTML 轉成 JSX
- [ ] 將 `class` 改成 `className`
- [ ] 將 `for` 改成 `htmlFor`
- [ ] 將 inline event handler 改成 React function
- [ ] 將重複 UI 區塊拆成 component
- [ ] 將樣式整理到 CSS 檔案
- [ ] 將圖片與靜態資源放到正確位置
- [ ] 保留原始 UI 風格
- [ ] 避免未經要求重新設計

---

## 3. JavaScript Refactor

- [ ] 將 DOM 操作改成 React state
- [ ] 將互動邏輯放進 component
- [ ] 將共用函式移到 `src/utils/`
- [ ] 移除未使用 script
- [ ] 移除不必要全域變數
- [ ] 確認 browser API 可正常使用

---

## 4. Vue to React

- [ ] 將 Vue component 轉成 React function component
- [ ] 將 `data()` 轉成 `useState`
- [ ] 將 `computed` 轉成 derived value 或 `useMemo`
- [ ] 將 `methods` 轉成 function
- [ ] 將 `v-if` 轉成 conditional rendering
- [ ] 將 `v-for` 轉成 `.map()`
- [ ] 將 `v-model` 轉成 controlled component
- [ ] 將 lifecycle hook 轉成 `useEffect`
- [ ] 移除 Vue dependency

---

## 5. Vite Setup

- [ ] 建立 `package.json`
- [ ] 建立 `vite.config.js`
- [ ] 建立 `index.html`
- [ ] 建立 `src/main.jsx`
- [ ] 建立 `src/App.jsx`
- [ ] 確認 `@vitejs/plugin-react` 已安裝
- [ ] 確認 scripts 包含 `dev`
- [ ] 確認 scripts 包含 `build`
- [ ] 確認 scripts 包含 `preview`

---

## 6. Build Verification

- [ ] 執行 `npm install`
- [ ] 執行 `npm run build`
- [ ] 修正 build error
- [ ] 確認產生 `dist/`
- [ ] 確認 CSS / JS / images 正常被打包
- [ ] 確認沒有 missing asset
- [ ] 確認沒有 secrets 被寫入專案

---

## 7. Deployment Profile

- [ ] 若部署到 IIS，套用 `ai-web-package-react-iis`
- [ ] 若部署到 Zeabur，套用 `ai-web-package-react-zeabur`
- [ ] 更新 README deployment section
- [ ] 加入必要部署設定檔
- [ ] 加入部署後驗證清單

---

## 8. Final Response

Codex 完成後需回覆：

- [ ] 轉換了什麼
- [ ] 套用了哪個 Profile
- [ ] 建立或修改了哪些檔案
- [ ] 如何本地執行
- [ ] 如何建置
- [ ] 如何部署
- [ ] 有哪些假設或待處理問題
