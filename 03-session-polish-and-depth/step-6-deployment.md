# Step 6: Deployment — Going Live

> Git, GitHub, Vercel. Your portfolio on the internet. ~25 minutes.

## 6.1 Initialize Git

In your terminal (make sure you're in the `my-portfolio/` directory):

```powershell
git init
git add .
git commit -m "Initial portfolio with chatbot and private tools"
```

### Verify Before Committing

```powershell
git status
```

**Check:** `.env.local` should NOT appear in the file list. If it does, your `.gitignore` is wrong.

## 6.2 Push to GitHub

### Option A: GitHub CLI

```powershell
gh repo create my-portfolio --public --push --source .
```

### Option B: Manual

1. Go to [github.com/new](https://github.com/new)
2. Repository name: `my-portfolio`
3. Public (so Vercel can access it)
4. Do NOT initialize with README (you already have one)
5. Copy the remote URL, then:

```powershell
git remote add origin https://github.com/YOUR-USERNAME/my-portfolio.git
git branch -M main
git push -u origin main
```

## 6.3 Deploy to Vercel

1. Go to [vercel.com](https://vercel.com) → Sign up with GitHub
2. Click **"Add New Project"**
3. Import your `my-portfolio` repository
4. Framework: **Next.js** (auto-detected)
5. **Environment Variables** — add these:

| Variable | Value |
|----------|-------|
| `ANTHROPIC_API_KEY` | `sk-ant-...` (your key) |
| `SITE_PASSWORD` | Your chosen password |

6. Click **Deploy**

### Wait for Build

Vercel runs `npm run build`. Watch the logs.

**If build fails:** Read the error message. It's usually the same errors from Step 5 of Session 2. Fix locally, commit, push — Vercel auto-redeploys.

## 6.4 Verify Your Live Site

Once deployed, Vercel gives you a URL like `my-portfolio-abc123.vercel.app`.

### Test Checklist

| Test | Expected | Pass? |
|------|----------|-------|
| Homepage loads | Dark theme, your content visible | ☐ |
| Profile image shows | Your photo, not broken image | ☐ |
| Chatbot works | Responds to questions | ☐ |
| `/dashboard` redirects to `/login` | Login page appears | ☐ |
| Login with password | Dashboard accessible | ☐ |
| Fit analyzer works | Score + analysis returned | ☐ |
| Resume tailor works | Tailored bullets returned | ☐ |
| Cover letter works | Letter generated | ☐ |
| Mobile responsive | Looks good on phone (resize browser) | ☐ |

## 6.5 Custom Domain (Optional)

If you bought a domain during pre-work:

1. Vercel → Project → Settings → Domains
2. Add your domain (e.g., `yourname.dev`)
3. Follow DNS instructions (usually: add CNAME record pointing to `cname.vercel-dns.com`)
4. Wait for DNS propagation (usually < 10 min, can take up to 48 hrs)

## 6.6 Troubleshooting

| Problem | Likely Cause | Fix |
|---------|-------------|-----|
| Build fails on Vercel | TypeScript error | Fix locally, push again |
| Chatbot returns 500 | `ANTHROPIC_API_KEY` not set in Vercel | Add env var in Vercel dashboard |
| Login doesn't work | `SITE_PASSWORD` not set in Vercel | Add env var in Vercel dashboard |
| Images broken | Wrong path in content.json | Should be `/profile.jpg`, not `./profile.jpg` |
| "Module not found" | Missing dependency | `npm install`, commit, push |

## Checklist

- [ ] Git initialized + committed
- [ ] Pushed to GitHub
- [ ] Deployed to Vercel with environment variables
- [ ] All 9 tests pass on live URL
- [ ] (Optional) Custom domain configured
