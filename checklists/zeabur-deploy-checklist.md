# Zeabur Deploy Checklist

## Local

- [ ] 執行 `npm install`
- [ ] 執行 `npm run dev`
- [ ] 執行 `npm run build`
- [ ] 確認產生 `dist/`

## GitHub

- [ ] 初始化 Git
- [ ] 建立 GitHub repository
- [ ] 推送到 GitHub
- [ ] 確認 GitHub 上有 `package.json`
- [ ] 確認 GitHub 上有 `vite.config.js`
- [ ] 確認 GitHub 上有 `zeabur.json`

## Zeabur

- [ ] 建立 Zeabur project
- [ ] 選擇 GitHub repository
- [ ] 確認 build command 為 `npm run build`
- [ ] 確認 output directory 為 `dist`
- [ ] 執行 Deploy
- [ ] 檢查 build log
- [ ] 開啟正式網址測試

## Common Issues

- [ ] 若 build failed，檢查 dependency
- [ ] 若畫面空白，檢查 Browser Console
- [ ] 若 asset 404，檢查 Vite `base`
- [ ] 若圖片失效，檢查 asset path
