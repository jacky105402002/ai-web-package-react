# ai-web-package-react

將 AI 工具產出的前端原型，標準化轉換成可維護、可建置、可部署的 React/Vite 靜態網站專案。

這個 repository 是給 Codex / Agents 使用的 Skill 文件包，目標是把 Gemini Canvas、Claude、v0 或其他 AI 工具產出的前端原型，整理成可自行部署的 React + Vite 靜態網站。

## Skill 架構

```txt
ai-web-package-react/
│
├─ SKILL.md
│
├─ profiles/
│  ├─ iis.md
│  └─ zeabur.md
│
├─ templates/
│  ├─ vite-react-package-json.md
│  ├─ iis-web-config.md
│  └─ zeabur-json.md
│
└─ checklists/
   ├─ conversion-checklist.md
   ├─ iis-deploy-checklist.md
   └─ zeabur-deploy-checklist.md
```

## 主 Skill

- `ai-web-package-react`

負責將 AI 工具產出的 HTML、CSS、JavaScript、React、Vue 或混合型前端專案，整理成標準 React + Vite 靜態網站。

## 部署 Profiles

目前支援：

- `ai-web-package-react-iis`
- `ai-web-package-react-zeabur`

未來可擴充：

- `docker`
- `nginx`
- `github-pages`
- `cloudflare-pages`
- `vercel`
- `netlify`
- `laravel-public`

## 建議安裝位置

Windows Codex / Agents skills 目錄建議放在：

```txt
C:\Users\JackyTsai\.agents\skills\ai-web-package-react
```

## 使用範例

Zeabur：

```txt
請使用 ai-web-package-react skill，將這個 AI 工具產出的前端原型轉換成 React + Vite 靜態網站專案，並套用 ai-web-package-react-zeabur profile。
```

IIS：

```txt
請使用 ai-web-package-react skill，將這個 AI 工具產出的前端原型轉換成 React + Vite 靜態網站專案，並套用 ai-web-package-react-iis profile。
```

## 設計原則

- 以靜態網站為主
- 預設使用 React + Vite
- 不主動加入 backend、database、authentication
- 不主動改成 Next.js
- 不主動重設計 UI
- 優先保留原始畫面與互動邏輯
- IIS 與 Zeabur 差異透過 profile 處理
