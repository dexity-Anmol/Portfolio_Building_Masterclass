# Step 3: Resume Tailor

> Generate tailored resume bullets for a specific job. ~15 minutes.

## 3.1 What It Does

```
INPUT:  Job description (paste from job posting)
OUTPUT: Tailored experience bullets optimized for that role
```

It rewrites your existing experience bullets (from `content.json`) to emphasize what matters for THIS specific job.

## 3.2 Build 2 Files

### File 1: API Route

```
Create src/app/api/resume-tailor/route.ts:

POST handler for resume tailoring.

BEHAVIOR:
- Accept { jobDescription: string }
- Read data/content.json for experience + skills
- Send to Claude with system prompt:
  "You are an expert resume writer. Given the candidate's
   experience and a target job description, rewrite each
   experience entry's bullets to emphasize relevant skills
   and achievements for this specific role.
   Rules:
   - Keep all facts accurate (don't invent metrics)
   - Each bullet: action verb + what + quantified result
   - Reorder bullets to put most relevant first
   - If an experience entry is irrelevant, keep 1 bullet max
   Return JSON: { tailoredExperience: [{ company, title,
   bullets: string[] }] }"
- max_tokens: 2000, model: claude-sonnet-4-20250514

VALIDATION:
- jobDescription must be string, max 5000 chars
- Rate limit: 5 requests/minute per IP (use keyed rate limiter)
```

### File 2: Page

```
Create src/app/dashboard/resume-tailor/page.tsx:

A page to tailor resume bullets.
- "use client"
- Large textarea for job description
- "Tailor Resume" button
- Loading state
- Results: show each job with original vs tailored bullets
  side by side (or before/after toggle)
- Copy button for tailored version
- Dark theme, responsive
- Back link to /dashboard
```

## 3.3 Test It

1. Paste the same job you used for fit analyzer
2. Compare tailored bullets to your original content.json
3. Check: Are facts preserved? Are relevant skills highlighted?

**Key test:** The tailored bullets should NOT contain skills or achievements that aren't in your original content.

## Checklist

- [ ] `src/app/api/resume-tailor/route.ts` — Claude-powered tailoring
- [ ] `src/app/dashboard/resume-tailor/page.tsx` — paste + compare UI
- [ ] Tested: facts preserved, relevance improved
- [ ] No invented metrics or fake experience
