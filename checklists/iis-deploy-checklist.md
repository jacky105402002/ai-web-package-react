# IIS Deploy Checklist

## Build

- [ ] 執行 `npm install`
- [ ] 執行 `npm run build`
- [ ] 確認產生 `dist/`
- [ ] 確認 `vite.config.js` 預設使用 `base: './'`，除非使用者明確指定固定根路徑
- [ ] 確認原始專案主要入口檔是哪一個，例如 `index.html`、`game.html`、`home.html`
- [ ] 確認正式部署首頁為 `dist/index.html`
- [ ] 若原始入口不是 `index.html`，確認 build 流程已將實際入口正規化成 `dist/index.html`
- [ ] 確認 `dist/index.html` 的 CSS / JS 路徑是 `./assets/...`，不是 `/assets/...`
- [ ] 確認 `web.config` 已準備好
- [ ] 確認 `web.config` 會被放進 IIS 部署目錄

## IIS

- [ ] 確認 IIS Site 已建立
- [ ] 確認 Site path 指向正確資料夾
- [ ] 確認 URL Rewrite 已安裝
- [ ] 確認 MIME type 正常
- [ ] 確認資料夾網址可以正常開啟，例如 `/my-app/`
- [ ] 確認首頁可開啟
- [ ] 確認重新整理不會 404
- [ ] 確認 CSS / JS / images 正常載入

## Common Issues

- [ ] 若畫面空白，檢查 Browser Console
- [ ] 若開資料夾網址空白或顯示外殼錯誤，檢查 `dist/index.html` 是否為正式入口
- [ ] 若真正入口是 `game.html`、`home.html` 等非 `index.html` 檔，確認已在 build 後複製或輸出成 `dist/index.html`
- [ ] 若 asset 404，優先檢查 Vite `base` 是否誤用 `base: '/'`
- [ ] 若分站或虛擬目錄部署時 `/assets/...` 404，改用 `base: './'` 後重新 build
- [ ] 若 route 404，檢查 `web.config`
- [ ] 若 JSON/SVG 失敗，檢查 MIME type
