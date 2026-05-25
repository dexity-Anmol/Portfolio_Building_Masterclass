# Session 3 Prompts — Copy & Paste Reference

> All prompts use Copilot Chat (`Ctrl+Shift+I`). Build files in order.

---

## Prompt 1: Middleware (Auth Gate)

```
Create src/middleware.ts:

Next.js middleware that protects /dashboard routes.
- Check for httpOnly cookie "auth-token"
- If valid: allow request
- If missing/invalid: redirect to /login
- Matcher: /dashboard, /dashboard/:path*,
  /api/fit-analyzer, /api/resume-tailor, /api/cover-letter
- Do NOT protect: /, /login, /api/chat, /api/auth

Export config with matcher array.
TypeScript, Next.js App Router middleware pattern.
```

---

## Prompt 2: Auth API Route

```
Create src/app/api/auth/route.ts:

LOGIN (POST { password }):
- Compare against process.env.SITE_PASSWORD
- Match: set httpOnly cookie "auth-token" (hashed value),
  secure, sameSite strict, maxAge 24h. Return { success: true }
- Wrong: return 401 { error: "Invalid password" }

LOGOUT (DELETE):
- Clear auth-token cookie. Return { success: true }
```

---

## Prompt 3: Login Page

```
Create src/app/login/page.tsx:
- "use client"
- Password input + Submit button
- POST to /api/auth with { password }
- Success: redirect to /dashboard
- Failure: show error message
- Dark theme, centered card layout
```

---

## Prompt 4: Dashboard Hub

```
Create src/app/dashboard/page.tsx:
- "use client"
- 3 cards: Fit Analyzer, Resume Tailor, Cover Letter Generator
- Each links to /dashboard/[tool-name]
- Logout button (DELETE to /api/auth → redirect to /)
- Dark theme, responsive grid
```

---

## Prompt 5: Fit Analyzer (API + Page)

```
Create 2 files for the Job Fit Analyzer:

FILE 1: src/app/api/fit-analyzer/route.ts
- POST { jobDescription }
- Read data/content.json for skills + experience
- Claude prompt: compare candidate vs job, return JSON:
  { score: 0-100, matchingSkills: [], missingSkills: [], verdict }
- max_tokens 500, claude-sonnet-4-20250514
- Validate input (string, max 5000 chars), rate limit

FILE 2: src/app/dashboard/fit-analyzer/page.tsx
- "use client"
- Textarea for job description + Analyze button
- Show: score (color-coded), matching/missing skills as tags, verdict
- Dark theme, back link to /dashboard
```

---

## Prompt 6: Resume Tailor (API + Page)

```
Create 2 files for the Resume Tailor:

FILE 1: src/app/api/resume-tailor/route.ts
- POST { jobDescription }
- Read data/content.json experience entries
- Claude prompt: rewrite bullets for this specific role.
  Keep facts accurate, action verb + metric format.
  Return JSON: { tailoredExperience: [{ company, title, bullets }] }
- max_tokens 1000, claude-sonnet-4-20250514
- Validate + rate limit

FILE 2: src/app/dashboard/resume-tailor/page.tsx
- "use client"
- Textarea + Tailor button
- Show original vs tailored bullets side by side
- Copy button for tailored version
- Dark theme, back link
```

---

## Prompt 7: Cover Letter (API + Page)

```
Create 2 files for the Cover Letter Generator:

FILE 1: src/app/api/cover-letter/route.ts
- POST { jobDescription, tone: "formal"|"conversational"|"technical" }
- Read data/content.json
- Claude prompt: write 3-4 paragraph cover letter in chosen tone.
  Reference specific experience + job requirements. No generic filler.
  Return JSON: { coverLetter: string, keyPoints: string[] }
- max_tokens 800, claude-sonnet-4-20250514
- Validate + rate limit

FILE 2: src/app/dashboard/cover-letter/page.tsx
- "use client"
- Textarea + tone selector (3 radio buttons) + Generate button
- Show letter + key points as tags + copy button
- Dark theme, back link
```

---

## Prompt 8: Color Reskin

```
Update my site's accent color from #3b82f6 to [YOUR COLOR].
Change in: globals.css CSS variables + any hardcoded values
in components. Show which files changed.
```

---

## Prompt 9: Security Audit

```
Audit my project for security:
1. Search .ts/.tsx for hardcoded API keys or passwords
2. Every API route has: input validation, rate limiting,
   safe error handling
3. middleware.ts protects all /dashboard routes
4. .env.local in .gitignore

Report as table: Check, PASS/FAIL, Details.
```
