# 理財目標規劃工具

以 PV（現值）、FV（終值）、PMT（定期投入）進行理財目標分析的純靜態網站，無需建置步驟、無外部依賴。

## 功能

- **目標分析**：輸入目標金額、年限、現有資產、每月投入、報酬率、通膨率，計算預估終值、目標缺口、達標所需月投入，並提供成長圖與情境比較。
- **PV / FV / PMT 計算機**：任選一項求解。
- 計算假設：月複利、每月月底投入。結果僅供規劃參考，不構成投資建議。

## 專案結構

```
finance-planner/
├── public/                 # 網站檔案（部署目錄）
│   ├── index.html
│   ├── favicon.svg
│   └── _headers            # Cloudflare 安全標頭
├── .github/workflows/
│   └── deploy.yml          # 選用：GitHub Actions 自動部署
├── wrangler.jsonc          # Cloudflare 設定（方法 C 使用）
├── package.json
├── .gitignore
├── LICENSE
└── README.md
```

## 本機預覽

直接用瀏覽器開啟 `public/index.html`，或：

```bash
cd public && python3 -m http.server 8000
# 開啟 http://localhost:8000
```

## 上傳到 GitHub

```bash
cd finance-planner
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<你的帳號>/finance-planner.git
git push -u origin main
```

（請先在 GitHub 建立空的 `finance-planner` repository，不要勾選 README。）

## 部署到 Cloudflare

### 方法 A：Cloudflare 後台連接 GitHub（最簡單，推薦）

1. 登入 Cloudflare Dashboard →「Workers & Pages」→「Create」→「Pages」→「Connect to Git」。
2. 授權並選擇 `finance-planner` repository。
3. 設定：
   - Framework preset：`None`
   - Build command：留空
   - Build output directory：`public`
4. 按「Save and Deploy」。之後每次推送到 `main` 都會自動部署，網址為 `https://finance-planner.pages.dev`（名稱若被占用會自動調整）。
5. 如需自訂網域：專案 →「Custom domains」→ 加入網域。

### 方法 B：GitHub Actions 自動部署

1. 先在 Cloudflare 建立 Pages 專案（Direct Upload，名稱 `finance-planner`），或讓首次部署自動建立。
2. 到 Cloudflare 建立 API Token（權限：Account → Cloudflare Pages → Edit），並取得 Account ID。
3. 在 GitHub repository →「Settings」→「Secrets and variables」→「Actions」新增：
   - `CLOUDFLARE_API_TOKEN`
   - `CLOUDFLARE_ACCOUNT_ID`
4. 推送到 `main` 即觸發 `.github/workflows/deploy.yml`。
5. 若改用方法 A，請刪除該 workflow，避免重複部署。

### 方法 C：Wrangler 指令列

```bash
npm install
npx wrangler login
npm run deploy
```

此方法使用 `wrangler.jsonc`，以 Workers Static Assets 部署 `public/` 目錄。

## 授權

MIT License
