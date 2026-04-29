# IIS Deploy Checklist

## Build

- [ ] 執行 `npm install`
- [ ] 執行 `npm run build`
- [ ] 確認產生 `dist/`
- [ ] 確認 `vite.config.js` 預設使用 `base: './'`，除非使用者明確指定固定根路徑
- [ ] 確認 `dist/index.html` 的 CSS / JS 路徑是 `./assets/...`，不是 `/assets/...`
- [ ] 確認 `web.config` 已準備好
- [ ] 確認 `web.config` 會被放進 IIS 部署目錄

## IIS

- [ ] 確認 IIS Site 已建立
- [ ] 確認 Site path 指向正確資料夾
- [ ] 確認 URL Rewrite 已安裝
- [ ] 確認 MIME type 正常
- [ ] 確認首頁可開啟
- [ ] 確認重新整理不會 404
- [ ] 確認 CSS / JS / images 正常載入

## Common Issues

- [ ] 若畫面空白，檢查 Browser Console
- [ ] 若 asset 404，優先檢查 Vite `base` 是否誤用 `base: '/'`
- [ ] 若分站或虛擬目錄部署時 `/assets/...` 404，改用 `base: './'` 後重新 build
- [ ] 若 route 404，檢查 `web.config`
- [ ] 若 JSON/SVG 失敗，檢查 MIME type
