# PLC Direct — Product Content Generator
## Deployment Guide

A Next.js web app that generates AI product content in bulk.  
Your API key lives securely on the server — never exposed to the browser.

---

## How to deploy (Vercel — free, ~5 minutes)

### Step 1 — Push to GitHub

1. Go to **github.com** and create a new repository (name it `plc-content-generator`)
2. Upload all these project files to the repository
   - Easiest way: drag the entire `plc-content-generator` folder into the GitHub web UI

### Step 2 — Deploy on Vercel

1. Go to **vercel.com** and sign in with your GitHub account (free)
2. Click **"Add New Project"**
3. Select your `plc-content-generator` repository
4. Vercel auto-detects Next.js — click **"Deploy"**
5. Wait ~2 minutes for the first deploy

### Step 3 — Add your API key (critical!)

1. In Vercel, go to your project → **Settings → Environment Variables**
2. Add this variable:
   - Name: `ANTHROPIC_API_KEY`
   - Value: `sk-ant-api03-...` (your key from console.anthropic.com)
   - Environment: ✓ Production ✓ Preview ✓ Development
3. Click **Save**
4. Go to **Deployments** → click the three dots on the latest deploy → **"Redeploy"**

### Step 4 — Share with your team

Your tool is now live at `https://your-project-name.vercel.app`

Share this URL with your team. That's it.

---

## Optional: Password protect the tool

If you want to restrict access to your team only:

1. In Vercel → Settings → Environment Variables, add:
   - Name: `TEAM_PASSWORD`
   - Value: `choose-a-team-password`
2. Redeploy
3. Tell your team to click **⚙ Settings** in the top-right of the app and enter the password before processing

The password is saved in each user's browser — they only need to enter it once.

---

## Optional: Custom domain

1. In Vercel → Settings → Domains
2. Add your domain, e.g. `products.plc-direct.com`
3. Update your DNS records as instructed by Vercel

---

## Local development

```bash
# Install dependencies
npm install

# Copy and fill in environment variables
cp .env.local.example .env.local
# Edit .env.local and add your ANTHROPIC_API_KEY

# Run locally
npm run dev

# Open http://localhost:3000
```

---

## Project structure

```
plc-content-generator/
├── app/
│   ├── layout.js           ← HTML shell, metadata
│   ├── page.jsx            ← Main batch tool UI
│   ├── globals.css         ← All styles
│   └── api/
│       └── generate/
│           └── route.js    ← Secure API proxy (holds API key)
├── package.json
├── next.config.mjs
├── .env.local.example      ← Copy to .env.local with your key
├── .gitignore
└── README.md
```

---

## How it works

```
Your team's browser
      │
      │  POST /api/generate { pn, brand, ctx }
      ▼
  Vercel server (Next.js API route)
      │
      │  Uses ANTHROPIC_API_KEY (env var, never sent to browser)
      ▼
  Anthropic API
      │
      ▼
  Returns generated product JSON
      │
      ▼
  Displayed + added to CSV download
```

The API key never leaves Vercel's servers. Your team's browsers only talk to your own `/api/generate` endpoint.

---

## Cost management

- **Vercel hosting**: Free (Hobby plan supports team usage)
- **Anthropic API**: ~$0.003–0.005 per product at current Sonnet pricing
  - 1,000 products ≈ $3–5
  - 50,000 products ≈ $150–250
  - Set a spend limit at console.anthropic.com → Billing

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Unauthorized" error | Enter team password in ⚙ Settings |
| Processing very slow | Increase concurrency slider (try 10–15) |
| Rate limit errors appearing | Reduce concurrency slider |
| White screen on load | Check Vercel deployment logs for build errors |
| API key not working | Redeploy after adding env var in Vercel |

---

## Updating the app

Any push to your GitHub `main` branch automatically redeploys on Vercel.
