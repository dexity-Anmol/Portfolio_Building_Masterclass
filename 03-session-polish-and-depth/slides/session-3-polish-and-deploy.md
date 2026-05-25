---
marp: true
theme: default
paginate: true
backgroundColor: #ffffff
color: #1e293b
header: '![w:100](Dexity%20Logo.png)'
style: |
  section {
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(180deg, #ffffff 0%, #f0f5ff 100%);
  }
  h1 {
    color: #1d4ed8;
    border-bottom: 3px solid #3b82f6;
    padding-bottom: 0.3em;
  }
  h2 {
    color: #1d4ed8;
  }
  h3 {
    color: #2563eb;
  }
  code {
    background: #e0e9ff;
    color: #1e3a5f;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre {
    background: #f0f4ff;
    border: 1px solid #bfdbfe;
    border-radius: 8px;
    font-size: 0.7em;
  }
  pre code {
    background: transparent;
    color: #1e3a5f;
  }
  a {
    color: #2563eb;
  }
  table {
    font-size: 0.85em;
    border-collapse: collapse;
    width: 100%;
  }
  th {
    background: #1d4ed8;
    color: #ffffff;
    padding: 8px 12px;
    border: 1px solid #3b82f6;
  }
  td {
    background: #ffffff;
    color: #1e293b;
    padding: 8px 12px;
    border: 1px solid #dbeafe;
  }
  tr:nth-child(even) td {
    background: #eff6ff;
  }
  blockquote {
    border-left: 4px solid #3b82f6;
    background: #eff6ff;
    padding: 0.5em 1em;
    border-radius: 0 8px 8px 0;
    color: #1e40af;
  }
  strong {
    color: #1d4ed8;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }
  .bad { color: #dc2626; }
  .good { color: #16a34a; }
  .highlight { color: #d97706; }
  .small { font-size: 0.7em; }
  header {
    box-sizing: border-box;
    text-align: right;
    padding: 15px 50px;
    width: 100%;
    overflow: hidden;
  }
  header img {
    opacity: 0.6;
  }
  section::after {
    color: #64748b;
    font-size: 0.7em;
  }
---

<!-- _header: '' -->

![w:250](Dexity%20Logo.png)

# Session 3: Your Private Tools & Going Live

## Cohort 0 — AI Portfolio Engineering

**Auth, job match, resume tailor, security, deploy.**
**Instructor:** Nahid Farady, PhD | **Live demo:** https://nahid-portfolio-delta.vercel.app

---

# Where We Left Off

From Sessions 1 & 2:

```
my-portfolio/
├── data/content.json           ✅ Structured content
├── public/profile.jpg          ✅ Your photo
├── src/app/page.tsx            ✅ Homepage with 6 sections
├── src/app/api/chat/           ✅ Public chatbot (hardened!)
├── src/components/             ✅ All UI sections + chat widget
└── .env.local                  ✅ Secrets
```

It builds. It runs. Your chatbot doesn't get jailbroken.

**But it's missing the private tools.** That's what makes this portfolio a **career weapon**, not just a pretty page.

---

# Advanced Prompt Engineering: Multi-File Features

## Session 1 taught the formula. Session 2 taught patterns. Now we compose.

---

# From Single Files to Features

In Sessions 1 & 2, you wrote **one prompt per file.** That works for independent pieces.

But features like auth, job fit analyzer, and resume tailor are **multi-file systems** — several files that must work together:

```
Feature: Job Fit Analyzer
├── src/app/api/fit-analyzer/route.ts    ← API (server-side)
├── src/app/dashboard/fit-analyzer/page.tsx  ← UI (client-side)
└── depends on: lib/rate-limit.ts, data/content.json, middleware.ts
```

**You need a new strategy: the Feature Blueprint.**

---

# The Feature Blueprint Process

Before writing ANY prompt for a new feature:

```
Step 1: DEFINE — What does the feature do? (1 sentence)
  → "Analyze how well my profile matches a job description"

Step 2: MAP — What files are needed?
  → API route (server logic) + Page (UI) + any shared utilities

Step 3: INTERFACE — What data flows between files?
  → Page sends { jobDescription } → API returns { score, matches, gaps }

Step 4: ORDER — Which file gets built first?
  → API route first (defines the contract) → Page second (consumes it)

Step 5: PROMPT — One prompt per file, referencing the interface
  → API prompt specifies exact request/response shape
  → Page prompt specifies it calls that exact API with that shape
```

**This is how professional engineers think.** Define the contract between parts BEFORE writing any code.

---

# Interface-First Prompting

The most powerful technique: **define the API contract, then prompt both sides to follow it.**

```
INTERFACE CONTRACT for Fit Analyzer:

Request:  POST /api/fit-analyzer
          Body: { jobDescription: string }  (max 5000 chars)
          Auth: middleware checks cookie "auth-token"

Response: { 
  score: number (0-100),
  verdict: string,
  summary: string,
  strongMatches: string[],
  gaps: string[],
  recommendations: string[]
}

Error: { error: string } with status 400 | 429 | 500
```

**Now every prompt for this feature references this contract:**
- API route prompt → "Return JSON matching this exact shape"
- Page prompt → "Expect JSON response in this exact shape"

**No mismatches. No "why is my page reading `result.data.score` when the API returns `result.score`?"**

---

# Multi-File Prompt Template

Use this template when building any new feature:

```
FEATURE: [name]
INTERFACE: [request shape] → [response shape]

Prompt 1 (API route):
"Create [path]. Auth check + rate limit. Accept [request].
 Call Claude with [system prompt]. Return [response shape].
 Error handling for [error cases]."

Prompt 2 (UI page):
"Create [path]. 'use client'. Form for [input].
 POST to [API path] with [request shape].
 Display [response fields]. Loading + error states.
 Dark theme matching dashboard."
```

**Apply this to every feature today:** Auth, Fit Analyzer, Resume Tailor, Cover Letter.

---

# Recognizing Prompt Patterns Across Features

Notice something? Every private tool follows the SAME pattern:

| Step | Fit Analyzer | Resume Tailor | Cover Letter |
|------|-------------|--------------|-------------|
| 1. Auth check | middleware | middleware | middleware |
| 2. Rate limit | 5/min (keyed) | 5/min (keyed) | 5/min (keyed) |
| 3. Input | { jobDescription } | { jobDescription } | { jobDescription, tone } |
| 4. Claude call | system prompt + content | system prompt + content | system prompt + content |
| 5. Parse response | JSON: score + matches | JSON: bullets per job | JSON: letter + points |
| 6. Return | JSON | JSON | JSON |

**Once you nail one, the others are copy-modify-paste.** Change the system prompt and response shape — the skeleton is identical.

**This is pattern recognition — the real skill behind prompt engineering.**

---

# Today's Agenda (Session 3 — 2 hours)

| Time | Step | Outcome |
|------|------|---------|
| 10 min | Add auth + login + middleware | Private section locked down |
| 5 min | 🎯 Exercise: Test auth flow | Verified redirect + cookie |
| 10 min | Build dashboard layout | Private tool navigation |
| 15 min | Build Job Fit Analyzer | AI-powered job matching |
| 10 min | 🎯 Exercise: Test with a REAL job posting | Real match score |
| 10 min | Build Resume Tailor | AI resume rewriter |
| 5 min | 🎯 Exercise: Tailor your resume | Tailored bullets in hand |
| 10 min | Build Cover Letter Generator | One-click cover letters |
| 5 min | 🎯 Exercise: Generate a cover letter | Ready-to-send letter |
| 10 min | Reskin with your colors | YOUR unique look |
| 5 min | 🎯 Exercise: Verify reskin | Consistent new theme |
| 15 min | Security audit (6 checks) | Production-ready |
| 5 min | 🎯 Exercise: Fix security gaps | All checks pass |
| 10 min | Deploy to Vercel | Live URL! |
| 5 min | 🎯 Exercise: Verify deployment | Everything works in prod |
| 5 min | 🎯 Exercise: Peer review | Final feedback |
| 5 min | What's next + LinkedIn post | Launch strategy |

---

# Step 1: Add Auth & Private Dashboard

## Protect your AI tools from the public.

---

# 1.1 — Why Auth?

The fit analyzer, resume tailor, and cover letter tools:
- **Cost money** per request (Claude API calls)
- **Process private data** (your resume details + job descriptions)
- **Should only be available to YOU**

**Solution:** Password login → sets httpOnly cookie → middleware checks cookie.

```
Browser → GET /dashboard
  → middleware.ts checks: has cookie "auth-token"?
    NO → 302 redirect to /login
    YES → pass through to dashboard/page.tsx
```

---

# 1.2 — Prompt: Middleware

```
Create src/middleware.ts:

- Import NextRequest, NextResponse from "next/server"
- Check for cookie named "auth-token" with hashed value of SITE_PASSWORD
- If cookie matches → allow through
- If no cookie or mismatch → redirect to /login
- Export config.matcher = ["/dashboard", "/dashboard/:path*",
  "/api/fit-analyzer", "/api/resume-tailor", "/api/cover-letter"]

Middleware protects BOTH pages and private API routes.
```

---

# 1.3 — Prompt: Login Page + Auth API

```
Create TWO files:

1. src/app/login/page.tsx:
   - 'use client'
   - Simple password input form
   - Calls POST /api/auth with { password }
   - On success → redirect to /dashboard
   - On failure → show "Invalid password" error
   - Dark themed, centered on page
   - Show "Private Tools" title

2. src/app/api/auth/route.ts:
   - POST handler (login):
     - Accept POST with { password: string }
     - Compare against process.env.SITE_PASSWORD
     - If match: set httpOnly cookie "auth-token" with hashed value
       maxAge: 24 hours, path: "/", sameSite: "strict", secure: true
     - If no match: return 401
   - DELETE handler (logout):
     - Clear the "auth-token" cookie by setting maxAge: 0
     - Return JSON { success: true }
   - NEVER log or expose the actual password value
```

---

# 1.4 — Login and Logout are in One File

The auth route handles both login (POST) and logout (DELETE) in a single file: `src/app/api/auth/route.ts`.

**Login:** POST with `{ password }` → sets cookie → `{ success: true }`
**Logout:** DELETE (no body) → clears cookie → `{ success: true }`

---

# 🎯 Exercise 1: Build & Test Auth Flow (5 min)

1. Generate: `middleware.ts`, `login/page.tsx`, `api/auth/route.ts`
2. Test the flow:
   - Visit http://localhost:3000/dashboard → should redirect to /login
   - Enter your password → should land on /dashboard
   - Refresh → should stay logged in (cookie persists)
3. Open an incognito window → `/dashboard` should redirect

**If login doesn't work:** Check terminal for errors. Common issues:
- `SITE_PASSWORD` not set in `.env.local`
- Cookie name mismatch between middleware and auth route
- Missing `'use client'` on login page

---

# Step 2: Build the Dashboard

## Your private tool hub.

---

# 2.1 — Prompt: Dashboard Layout & Overview

```
Create TWO files:

1. src/app/dashboard/layout.tsx:
   - "use client" layout wrapper for all dashboard pages
   - Tab navigation: Overview, Fit Analyzer, Resume Tailor, Cover Letter
   - Active tab highlighted in blue (#3b82f6)
   - Logout button that calls DELETE /api/auth then redirects to /
   - Dark theme, padded container

2. src/app/dashboard/page.tsx:
   - Dashboard overview page
   - Welcome message: "Your Private AI Tools"
   - Grid of 3 cards: Fit Analyzer, Resume Tailor, Cover Letter
   - Each card: emoji icon, title, one-line description, link
   - Dark cards with subtle borders and hover effect
```

---

# Step 3: Build the Job Fit Analyzer

## "Am I a good match for this role?" — answered with AI.

---

# 3.1 — What It Does

You paste (or upload) a job description. The AI:

1. Reads your entire `content.json` profile
2. Compares your experience against the JD requirements
3. Returns a **match score** (0-100) with specific strengths and gaps

**This is the tool you use BEFORE applying.** It tells you if it's worth your time and what to emphasize.

---

# 3.2 — Prompt: Fit Analyzer API Route

```
Create src/app/api/fit-analyzer/route.ts:

REQUIREMENTS:
- Rate limit: 5 requests per minute per IP (keyed: "fit:<ip>")
- Accept POST with JSON body: { jobDescription: string }
- Validate: jobDescription required, max 5000 characters
- Import content from data/content.json
- Call Claude API with system prompt that:
  - Takes the full candidate profile as context
  - Analyzes fit against the job description
  - Returns JSON: { score: 0-100, verdict: string,
    summary: string, strongMatches: string[],
    gaps: string[], recommendations: string[] }
- Return the parsed analysis as JSON
- Try/catch with error response

MODEL: claude-sonnet-4-20250514, max_tokens: 2000
```

---

# 3.3 — Prompt: Fit Analyzer Page

```
Create src/app/dashboard/fit-analyzer/page.tsx:

- 'use client'
- Large textarea for pasting job description
- "Analyze Fit" button → POST to /api/fit-analyzer with { jobDescription }
- While loading: disable button, show "Analyzing..."
- Display results:
  - Score as large number, color-coded: green (≥75), yellow (50-74), red (<50)
  - Verdict (one sentence) under the score
  - Summary paragraph
  - Strong Matches list (green tags)
  - Gaps list (orange tags)
  - Recommendations list (blue bullets)
- Error handling: show error message if request fails
- Dark theme matching dashboard
```

---

# 🎯 Exercise 2: Test with a REAL Job Posting (10 min)

This is where it gets real.

1. **Go to LinkedIn** (or any job board) right now
2. **Find a job you'd actually apply to** — copy the full description
3. **Paste it** into your Fit Analyzer
4. **Click Analyze** — wait for results

**What to look at:**
- [ ] Score feels accurate? (Be honest with yourself)
- [ ] Strong matches reference YOUR actual experience?
- [ ] Gaps are real things you'd need to address?
- [ ] Recommendations are actionable?

**Now try a job WAY outside your field** (nursing, cooking, law). Score should be very low.

**Discuss with your neighbor:** What did your score say? Do you agree?

---

# Step 4: Build the Resume Tailor

## Same resume, different bullets for every application.

---

# 4.1 — What It Does

You paste a job description. The AI:
1. Reads your experience from `content.json`
2. Rewrites your bullet points to **emphasize what this specific role needs**
3. Returns tailored bullets, suggested order, and keywords to include

**This is NOT fabrication.** It's reorganizing and reframing YOUR real experience.

---

# 4.2 — Prompt: Resume Tailor API + Page

```
Create src/app/api/resume-tailor/route.ts:

- Rate limit: 5/minute per IP (keyed: "resume:<ip>")
- Accept POST: { jobDescription: string }
- Validate: required, max 5000 chars
- Import content from data/content.json
- System prompt: "Given this candidate profile and job description,
  rewrite the resume bullets to emphasize relevant experience.
  Return JSON: { tailoredExperience: [{ company, title,
  bullets: string[] }] }"
- Use claude-sonnet-4-20250514, max_tokens: 2000
- Try/catch, return JSON response

Create src/app/dashboard/resume-tailor/page.tsx:
- 'use client'
- Textarea for job description (5000 char limit)
- "Tailor My Resume" button → POST /api/resume-tailor
- Display: tailored bullets per job entry (company, title, bullets)
- Loading state + error handling
- Dark theme
```

---

# 🎯 Exercise 3: Tailor Your Resume (5 min)

1. Use the **same job posting** from Exercise 2
2. Paste it into Resume Tailor
3. Click "Tailor My Resume"
4. **Compare** the tailored bullets to your original content.json bullets

**Questions to ask yourself:**
- Are the tailored bullets still TRUE? (AI shouldn't invent)
- Do they emphasize the RIGHT skills for this role?
- Would you actually use these on a real application?

**Copy the best bullets** — you might need them tonight.

---

# Step 5: Build the Cover Letter Generator

## One click. Personalized. Professional.

---

# 5.1 — Prompt: Cover Letter API + Page

```
Create src/app/api/cover-letter/route.ts:
- Same rate limit pattern (keyed: "cover:<ip>", 5/min)
- Accept: { jobDescription: string, tone?: "formal"|"conversational"|"technical" }
- Generate a cover letter referencing specific candidate experience
- Return: { coverLetter: string, keyPoints: string[] }

Create src/app/dashboard/cover-letter/page.tsx:
- 'use client'  
- Textarea for job description (5000 char limit)
- Tone toggle: Formal / Conversational / Technical (three buttons)
- "Generate Cover Letter" button → POST /api/cover-letter
- Display: formatted cover letter
- Show key points highlighted below the letter
- Loading state + error handling
- Dark theme
```

---

# 🎯 Exercise 4: Generate a Cover Letter (5 min)

1. Same job posting, one more time
2. Generate with **Formal** tone
3. Generate again with **Technical** tone
4. **Compare:** Which sounds more like you?

**Quick quality check:**
- [ ] References YOUR specific experience (not generic)?
- [ ] Mentions the company/role from the JD?
- [ ] No "passionate" or "proven track record"?
- [ ] Would you send this with minor edits?

**Copy the one you like.** You might actually use it.

---

# 🎯 Exercise 5: The Full Job Application Flow (5 min)

You now have a complete AI-assisted application pipeline. Try the full flow:

1. **Fit Analyzer** → "Am I a 75%+ match?" → Yes? Continue.
2. **Resume Tailor** → Copy the tailored bullets into your resume
3. **Cover Letter** → Copy, make 1-2 personal edits, ready to send

**Time this.** How long did the full flow take?

| Traditional | With your tools |
|-------------|----------------|
| 45-60 min per application | ~5 min per application |

**This is why the private dashboard exists.** It's not a demo — it's a productivity tool.

---

# 🎯 Exercise 6: Compare API Route Patterns (5 min)

Open these 3 files side by side:
- `src/app/api/chat/route.ts`
- `src/app/api/fit-analyzer/route.ts`
- `src/app/api/resume-tailor/route.ts`

**Answer these questions:**

| Question | chat | fit-analyzer | resume-tailor |
|----------|------|-------------|--------------|
| Auth check? | No (public) | Yes (middleware) | ? |
| Rate limit per min? | 20 | 5 | ? |
| Max input chars? | 500 | 5000 | ? |
| Model used? | claude-sonnet | claude-sonnet | ? |
| max_tokens? | 500 | 2000 | ? |

**Why are the limits different?** Chat is public + short answers. Tools are private + longer output.

**Pattern recognition:** All routes follow the same structure:
1. Rate limit (keyed by route + IP) → 2. Input validation → 3. Claude API call → 4. Return JSON → 5. Error handling

**Once you understand the pattern, you can build any new tool.**

---

# Step 6: Reskin with Your Colors

## Make it look like YOU, not a template.

---

# 6.1 — The Reskin Prompt Template

Pull out those 3 hex codes from Session 1:

```
I want to change my portfolio's color scheme.

CURRENT COLORS (find and replace everywhere):
- Background: #0a0a0f → [YOUR NEW BG]
- Text: #ededed → [YOUR NEW TEXT COLOR]  
- Accent: #3b82f6 → [YOUR NEW ACCENT]
- Card/surface: rgba(255,255,255,0.05) → [YOUR NEW CARD BG]

FILES TO UPDATE:
- src/app/globals.css (body styles)
- src/components/Hero.tsx
- src/components/Experience.tsx
- src/components/Skills.tsx
- src/components/Projects.tsx
- src/components/Education.tsx
- src/components/Contact.tsx
- src/components/ChatAssistant.tsx
- src/app/dashboard/layout.tsx

DO NOT change: layout, spacing, font sizes, animations, structure.
ONLY change: color values.
```

---

# 🎯 Exercise 7: Reskin & Verify (5 min)

1. Apply the reskin prompt
2. Run `npm run dev` → scroll through all sections
3. Check:
   - [ ] All sections use new colors?
   - [ ] Chat widget matches new theme?
   - [ ] Dashboard pages match too?
   - [ ] No section still using old blue (#3b82f6)?
4. Fix any missed elements with a targeted prompt

---

# Step 7: Security Audit

## The 6 checks that MUST pass before deployment.

---

# 7.1 — The 6-Point Checklist

| # | Check | How to Verify |
|---|-------|---------------|
| 1 | **Secrets hidden** | Search all `.tsx`/`.ts` files for "sk-ant" or "ANTHROPIC" |
| 2 | **Auth works** | Visit /dashboard without logging in → redirects? |
| 3 | **API auth works** | DevTools Console → `fetch('/api/fit-analyzer', {method:'POST', body:'{}'})` → 401? |
| 4 | **Rate limit** | Send 25 rapid chat messages → get "rate limited" |
| 5 | **Input validated** | Paste 10,000 characters → should get error |
| 6 | **.env.local safe** | Run `git status` → .env.local should NOT appear |

---

# 7.2 — Test Auth Enforcement

**Page auth:**
1. Open a new incognito window
2. Go to `http://localhost:3000/dashboard`
3. Should redirect to `/login` ✅

**API auth:**
Open DevTools Console (F12) and type:
```javascript
fetch('/api/fit-analyzer', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({jobDescription: 'test'})
}).then(r => console.log(r.status))
```
Should redirect (302) or fail — middleware blocks unauthenticated requests ✅

---

# 7.3 — Test Rate Limit & Validation

**Rate limit:**
```javascript
for(let i=0; i<25; i++) {
  fetch('/api/chat', {method:'POST',
    headers:{'Content-Type':'application/json'},
    body:JSON.stringify({message:'test'})
  }).then(r => console.log(i, r.status))
}
```
After ~20 requests → `429` responses.

**Input validation:**
```javascript
fetch('/api/chat', {method:'POST',
  headers:{'Content-Type':'application/json'},
  body:JSON.stringify({message: 'A'.repeat(10000)})
}).then(r => console.log(r.status))
```
Should return `400` (not 200).

---

# 🎯 Exercise 8: Run Full Security Audit (10 min)

Run ALL 6 checks. Track results:

- [ ] Check 1: No secrets in client code
- [ ] Check 2: /dashboard redirects without auth
- [ ] Check 3: /api/fit-analyzer blocked by middleware without cookie
- [ ] Check 4: Rate limit triggers after 20 requests
- [ ] Check 5: Oversized input rejected with 400
- [ ] Check 6: .env.local not in git

**If any fail → fix NOW:**

```
# Example: API route not rate-limited
In src/app/api/resume-tailor/route.ts, make sure it calls:

const { allowed } = checkRateLimit(`resume:${ip}`, 5);
if (!allowed) {
  return NextResponse.json({ error: 'Too many requests' }, { status: 429 });
}
```

---

# 🎯 Exercise 9: Mobile Quick Check (5 min)

1. Open DevTools (F12) → device toggle → set width to **375px**
2. Scroll through your entire homepage
3. Check:
   - [ ] Profile picture isn't overflowing
   - [ ] Navigation hamburger menu works
   - [ ] Cards stack correctly (1 column)
   - [ ] Chat widget doesn't cover content
   - [ ] Text is readable (not too small)
4. Fix the worst issue with a targeted prompt

---

# Step 8: Deploy to Vercel

## Go live in 5 minutes.

---

# 8.1 — Initialize Git & Push to GitHub

```powershell
git init
git add -A
git commit -m "Portfolio v1: ready to deploy"

# Create repo on github.com first (green "New" button), then:
git remote add origin https://github.com/YOUR-USERNAME/my-portfolio.git
git branch -M main
git push -u origin main
```

**Verify:** Go to your GitHub repo URL → files visible EXCEPT `.env.local`.

---

# 8.2 — Deploy on Vercel

1. Go to **vercel.com** → Log in
2. Click **"Add New Project"**
3. **Import** your GitHub repo
4. Vercel auto-detects Next.js — default settings are fine
5. **Environment Variables** (critical!):
   - Add `ANTHROPIC_API_KEY` = your key
   - Add `SITE_PASSWORD` = your password
6. Click **Deploy**
7. Wait ~2 minutes → you get a **live URL!** 🎉

---

# 8.3 — Instructor's Live Demo

**Live site:** https://nahid-portfolio-delta.vercel.app

---

# 🎯 Exercise 10: Deploy & Verify (10 min)

1. Push to GitHub
2. Import to Vercel
3. Set environment variables
4. Deploy
5. Test on YOUR LIVE URL:

| Test | Expected |
|------|----------|
| Homepage loads | All sections + profile pic visible |
| Chat works | Ask a question, get response |
| /dashboard redirects | Shows login page |
| Login works | Enter password → see dashboard |
| Fit analyzer works | Submit JD → get score |
| Resume tailor works | Submit JD → get tailored bullets |
| Mobile works | Open on actual phone |

6. **Share your URL in the group!** 🎉

---

# 8.4 — Deployment Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Build fails on Vercel | Different Node version | Set Node 18 in Vercel settings |
| Chat returns 500 | Missing ANTHROPIC_API_KEY | Add in Vercel dashboard |
| Login always fails | Missing SITE_PASSWORD | Add in Vercel dashboard |
| "Module not found" | Import path works locally but not in CI | Fix to use `@/` alias paths |
| Profile pic missing | Not in `public/` folder | Move image to `public/profile.jpg` |

**After fixing:** Vercel auto-redeploys on push. Just `git push` again.

---

# 🎯 Exercise 11: Peer Review (10 min)

Swap your Vercel URL with a partner. Test their live site:

**Content check:**
- [ ] Profile picture shows? Pitch has numbers?
- [ ] Experience bullets start with verbs?
- [ ] Skills include gaps (honest)?

**Security check:**
- [ ] Can you access /dashboard without password?
- [ ] Try a chatbot jailbreak — does it hold?

**Tools check (if they share their password):**
- [ ] Fit analyzer returns a score?
- [ ] Resume tailor produces real bullets?

**UX check:**
- [ ] Open on your phone — does it work?
- [ ] Chat has loading feedback?
- [ ] Navigation works on mobile?

**Rate their portfolio 1-5:**
| Category | Score |
|----------|-------|
| Content quality | /5 |
| Visual design | /5 |
| Chatbot behavior | /5 |
| Private tools | /5 |
| Mobile UX | /5 |

**Write 3 improvements** → share with your partner.

---

# What You Built: Final File Tree

```
my-portfolio/
├── data/
│   ├── content.json              ← Your structured identity
│   └── system-prompt.txt         ← Chatbot persona
├── public/
│   └── profile.jpg               ← Your photo
├── src/
│   ├── app/
│   │   ├── layout.tsx            ← Site wrapper
│   │   ├── page.tsx              ← Homepage
│   │   ├── globals.css           ← Your custom theme
│   │   ├── login/page.tsx        ← Auth form
│   │   ├── dashboard/
│   │   │   ├── layout.tsx        ← Tab navigation + logout
│   │   │   ├── page.tsx          ← Tool overview
│   │   │   ├── fit-analyzer/     ← Job match tool
│   │   │   ├── resume-tailor/    ← Resume rewriter
│   │   │   └── cover-letter/     ← Letter generator
│   │   └── api/
│   │       ├── auth/             ← Login + logout (POST/DELETE)
│   │       ├── chat/             ← PUBLIC chatbot
│   │       ├── fit-analyzer/     ← PRIVATE (middleware)
│   │       ├── resume-tailor/    ← PRIVATE (middleware)
│   │       └── cover-letter/     ← PRIVATE (middleware)
│   ├── components/
│   │   ├── Hero.tsx
│   │   ├── Experience.tsx
│   │   ├── Skills.tsx
│   │   ├── Projects.tsx
│   │   ├── Education.tsx
│   │   ├── Contact.tsx
│   │   └── ChatAssistant.tsx
│   ├── lib/rate-limiter.ts
│   └── middleware.ts
├── .env.local                    ← Secrets (never committed)
└── package.json
```

---

# Portfolio Maintenance Plan

Your portfolio is live. Keep it alive.

| Trigger | What to Update | How |
|---------|---------------|-----|
| New job/role | content.json → experience + hero | Edit JSON, git push, Vercel auto-deploys |
| New project | content.json → projects | Add entry with impact + technologies |
| New skill learned | content.json → skills | Move from gaps → moderate → strong |
| Applying to a job | Dashboard → fit analyzer + resume tailor | Use your private tools! |
| Got feedback | System prompt / UI | Apply the specific fix |

---

# Course Recap: 3 Sessions → 1 Portfolio

| Session | What You Built | Key Skills |
|---------|---------------|------------|
| **1: Your Story** | content.json, persona, profile pic, tagline | Constrained prompting, schema design, personal branding |
| **2: Build & Talk** | Homepage, chatbot, adversarial testing | Architecture, component building, AI security |
| **3: Private Tools & Deploy** | Auth, fit analyzer, resume tailor, cover letter, deployment | Auth patterns, API design, security audit, shipping |

---

# The Key Principle

**You are the engineer. AI is the tool.**

- You designed the schema → AI filled it
- You specified constraints → AI followed them
- You reviewed every file → AI generated the draft
- You tested security → AI wrote the code
- You decided what to build → AI executed

**This is not vibecoding. This is AI-assisted engineering.**

---

# What You Can Explain Now

1. "Where does your content live?" → `data/content.json`
2. "How does auth work?" → Middleware checks cookie on `/dashboard/*` and private API routes
3. "Is the API key safe?" → Only in `.env.local`, only server-side
4. "What prevents abuse?" → Rate limiting on all endpoints
5. "How do I apply to a job?" → Fit analyzer → Resume tailor → Cover letter
6. "Why 'use client'?" → Components using state/events need browser APIs

---

# What's Next (On Your Own)

**Add more features with the same process:**

```
1. DEFINE   — What does it do? (1 sentence)
2. ACCESS   — Public or private? (auth required?)
3. MAP      — What files? API route + page + utilities?
4. INTERFACE — Request shape → response shape
5. PROMPT   — One per file, using RCCFA formula:
              Role, Context, Constraints, Format, Anti
6. REVIEW   — Auth? Rate limit? Validation? Error handling?
7. TEST     — Works? Mobile? Edge cases?
```

---

# The Full Prompt Engineering Journey

Here's everything you learned across 3 sessions:

| Session | Prompt Skill | What You Practiced |
|---------|-------------|-------------------|
| **1** | RCCFA formula | Role + Context + Constraints + Format + Anti |
| **1** | 7 strategies | Specificity, constraints, examples, roles, anti, format, iterate |
| **1** | Constraint design | Writing measurable rules for AI output |
| **1** | Anti-constraints | Blocking AI bad habits explicitly |
| **2** | One-prompt-per-file | Focused generation with exact paths |
| **2** | Component prompts | Data shape + styling + responsive specs |
| **2** | Fix prompts | Targeted single-line repairs |
| **2** | Chain prompts | Utility → API → UI in dependency order |
| **2** | System prompts | Persona + boundaries + security rules |
| **3** | Feature blueprints | Define → Map → Interface → Order → Prompt |
| **3** | Interface-first | API contract before any code |
| **3** | Pattern reuse | Same skeleton, different system prompt |
| **3** | Multi-file composition | Coordinated prompts across files |

**You now have a complete prompt engineering toolkit.** Use it for any AI-assisted project.

**Ideas:**
- Blog (MDX pages)
- Interview prep tool (mock questions from JD)
- Analytics (Vercel Analytics — free)
- Dark/light mode toggle

---

# Share Your Work

Your portfolio is live. Now tell people:

**LinkedIn post template:**
> Just shipped my AI-powered portfolio in 3 sessions with @Dexity.
> It has a chatbot that answers questions about my experience,
> plus private AI tools that tailor my resume per job application.
> 
> [Your Vercel URL]
> 
> Built with Next.js, TypeScript, and Claude API.
> Every file written with constrained prompts — not vibecoding.

**Put the URL on:**
- Your resume (Skills section or header link)
- LinkedIn profile (Featured section)
- GitHub profile README
- Email signature

---

# Thank You 🎓

**Your portfolio is live. Your private tools work. Your chatbot holds.**

The process matters more than the product.
Next time you build anything with AI — use the same flow:
**Design → Constrain → Generate → Review → Secure → Ship.**

*Dexity · Cohort 0 · May 2026*
