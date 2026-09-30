# ☁️ FileShare

A simple, fast batch file-sharing service that runs entirely on **Cloudflare Workers** with **KV** storage. Upload one or many files, get a shareable link, and let them expire automatically.

Created by **Hossein Seyed Bagheri**.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Cloudflare Workers](https://img.shields.io/badge/Cloudflare-Workers-F38020?logo=cloudflare&logoColor=white)](https://workers.cloudflare.com/)

## ✨ Features

- 📤 Multi-file (batch) upload with a clean web interface
- 🔗 One shareable link per batch, plus individual links per file
- 👁️ Built-in preview page, raw inline view, and direct download
- ⏳ Expiry options: 1 day, 7 days, 30 days, or never
- 🌍 Unicode filename support and CORS headers
- 🚫 No servers, no database – just a Worker and one KV namespace

## 🧭 Routes

| Route | Description |
|---|---|
| `/` or `/upload` | Upload interface |
| `POST /api/upload` | Multi-file upload API (`file` fields + `expiry`) |
| `/batch/:id` | Batch page listing all files |
| `/view/:id` | File preview page |
| `/raw/:id` | File served inline |
| `/download/:id` | File served as attachment |

## ⚠️ Limits

- **18 MB per file** – KV values max out at 25 MB, and base64 encoding adds ~33% overhead.
- KV free tier includes 100 MB of total storage.
- For larger files or heavy use, consider migrating to [R2](https://developers.cloudflare.com/r2/).

## 🚀 Deployment

### 1. Get the code on GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<your-username>/fileshare.git
git push -u origin main
```

### 2. Create the KV namespace

```bash
npm install
npx wrangler login
npx wrangler kv namespace create FILES
```

Copy the returned `id` into `wrangler.toml`, replacing `REPLACE_WITH_YOUR_KV_NAMESPACE_ID`, then commit and push.

### 3. Link GitHub to Cloudflare (pick one)

#### Option A – Cloudflare Workers Builds (easiest)

1. Open the [Cloudflare dashboard](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Import a repository**.
2. Connect your GitHub account and select this repository.
3. Set the deploy command to `npx wrangler deploy` and save.

Every push to `main` now deploys automatically. If you go with this option, you can delete `.github/workflows/deploy.yml`.

#### Option B – GitHub Actions

The included workflow (`.github/workflows/deploy.yml`) deploys on every push to `main`. Add two secrets under **Repo → Settings → Secrets and variables → Actions**:

| Secret | Where to find it |
|---|---|
| `CLOUDFLARE_API_TOKEN` | Cloudflare → My Profile → API Tokens → *Edit Cloudflare Workers* template |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare dashboard → Workers & Pages → right sidebar |

### 4. (Optional) Custom domain

Cloudflare dashboard → your Worker → **Settings** → **Domains & Routes** → **Add** → **Custom Domain**.

## 💻 Local development

```bash
npm install
npm run dev
```

Then open http://localhost:8787.

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## 📄 License

Released under the [MIT License](LICENSE).

Copyright © 2026 Hossein Seyed Bagheri
