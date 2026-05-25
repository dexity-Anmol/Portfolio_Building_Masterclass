# Step 2: Job Fit Analyzer

> Paste a job description, get a match score with evidence. ~15 minutes.

## 2.1 What It Does

```
INPUT:  Job description (paste from job posting)
OUTPUT: Match score (0-100), verdict, summary, strong matches,
        gaps, and actionable recommendations
```

**This is YOUR private tool** — behind auth, for your eyes only.

## 2.2 Build 2 Files

### File 1: API Route

```
Create src/app/api/fit-analyzer/route.ts:

POST handler for job fit analysis.

BEHAVIOR:
- Accept { jobDescription: string }
- Read data/content.json for user's skills + experience
- Send to Claude with this system prompt:
  "You are a job fit analyzer. Compare this candidate's profile
   against the job description. Return JSON with:
   { score: number (0-100), verdict: string (one sentence),
     summary: string (2-3 sentences),
     strongMatches: string[], gaps: string[],
     recommendations: string[] }"
- max_tokens: 2000, model: claude-sonnet-4-20250514
- Rate limit: 5 requests/minute per IP (use keyed rate limiter)

VALIDATION:
- jobDescription must be string, max 5000 chars
- Rate limit by IP
- 400 for invalid input, 500 for API error
```

### File 2: Page

```
Create src/app/dashboard/fit-analyzer/page.tsx:

A page to analyze job fit.
- "use client"
- Large textarea for pasting job description
- "Analyze Fit" button
- Loading state while API processes
- Results display:
  - Score as large number with color
    (green ≥70, yellow 40-69, red <40)
  - Verdict (one sentence) under the score
  - Summary paragraph
  - Strong matches as green tags
  - Gaps as orange tags
  - Recommendations as bulleted list
- Dark theme, responsive
- Back link to /dashboard
```

## 2.3 Test It

1. Log in → go to `/dashboard/fit-analyzer`
2. Paste a real job description from LinkedIn/Indeed
3. Click Analyze
4. Check: Does the score make sense given your actual skills?

**Try 3 different jobs:** one perfect match, one stretch, one totally wrong. The scores should differ significantly.

## Checklist

- [ ] `src/app/api/fit-analyzer/route.ts` — Claude-powered analysis
- [ ] `src/app/dashboard/fit-analyzer/page.tsx` — paste + results UI
- [ ] Tested with 3 different job descriptions
- [ ] Scores are reasonable (not all 85%)
