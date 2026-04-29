# IIS Deploy Checklist

## Build

- [ ] 執行 `npm install`
- [ ] 執行 `npm run build`
- [ ] 確認產生 `dist/`
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
- [ ] 若 asset 404，檢查 Vite `base`
- [ ] 若 route 404，檢查 `web.config`
- [ ] 若 JSON/SVG 失敗，檢查 MIME type
