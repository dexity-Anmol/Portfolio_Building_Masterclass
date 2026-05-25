# Step 5: Reskin & Security Audit

> Make it yours visually + verify security before going live. ~15 minutes.

## 5.1 Quick Reskin (~5 min)

Your site currently uses the default dark theme. Pick YOUR accent color.

In Copilot Chat:

```
Update my site's accent color from #3b82f6 (blue) to [YOUR COLOR].

Change it in:
1. src/app/globals.css (CSS custom properties)
2. Any component that hardcodes #3b82f6

My preferred accent color: [pick one]
- Emerald: #10b981
- Purple: #8b5cf6
- Orange: #f97316
- Rose: #f43f5e
- Cyan: #06b6d4
- Or custom: #______

Find and replace all instances. Show me which files changed.
```

**Optional enhancements** (if time allows):
- Custom favicon
- Smooth scroll animations on section transitions
- Hover effects on project cards

## 5.2 Security Audit (~10 min)

Before deploying, run through this 6-point checklist:

### The 6-Point Security Audit

| # | Check | How to Verify | Pass? |
|---|-------|--------------|-------|
| 1 | **No secrets in code** | Search all `.ts`/`.tsx` files for `sk-ant`, passwords, keys | ☐ |
| 2 | **API keys in .env.local only** | `ANTHROPIC_API_KEY` and `SITE_PASSWORD` only in `.env.local` | ☐ |
| 3 | **.env.local in .gitignore** | Open `.gitignore`, verify `.env.local` is listed | ☐ |
| 4 | **Rate limiting on all AI routes** | `/api/chat`, `/api/fit-analyzer`, `/api/resume-tailor`, `/api/cover-letter` all call rate limiter | ☐ |
| 5 | **Input validation on all routes** | All POST routes validate input length and type | ☐ |
| 6 | **Auth on private routes** | `/dashboard/*` redirects to `/login` without cookie | ☐ |

### Automated Check

In Copilot Chat:

```
Audit my project for security issues:

1. Search all .ts and .tsx files for hardcoded strings that
   look like API keys (sk-ant-*), passwords, or secrets
2. Verify every API route in src/app/api/ has:
   - Input validation (type + length check)
   - Rate limiting
   - Error handling that doesn't leak internal details
3. Verify middleware.ts protects all /dashboard routes
4. Verify .env.local is in .gitignore

Report as a table: Check, Status (PASS/FAIL), Details.
```

### Fix Any Failures

For each FAIL, write a targeted fix prompt:

```
Security issue: [what's wrong] in [file].
Fix it by [specific change needed].
Change ONLY what's needed for this fix.
```

## Checklist

- [ ] Accent color changed to personal preference
- [ ] 6-point security audit completed
- [ ] All checks pass
- [ ] Any failures fixed and re-verified
