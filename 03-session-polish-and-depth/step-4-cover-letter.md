# Step 4: Cover Letter Generator

> Generate a cover letter with tone toggle. ~10 minutes.

## 4.1 What It Does

```
INPUT:  Job description + tone selection (formal / conversational / technical)
OUTPUT: Cover letter tailored to the role in your chosen voice
```

## 4.2 Build 2 Files

### File 1: API Route

```
Create src/app/api/cover-letter/route.ts:

POST handler for cover letter generation.

BEHAVIOR:
- Accept { jobDescription: string, tone: string }
- tone must be one of: "formal", "conversational", "technical"
- Read data/content.json for experience + skills
- Send to Claude with system prompt:
  "You are a cover letter writer. Write a cover letter for this
   candidate applying to the described role.
   Tone: [formal/conversational/technical]
   Rules:
   - 3-4 paragraphs
   - Reference specific experience from the candidate's profile
   - Reference specific requirements from the job description
   - No generic filler ('I am writing to express my interest...')
   - End with a specific call-to-action
   Return JSON: { coverLetter: string, keyPoints: string[] }"
- max_tokens: 800, model: claude-sonnet-4-20250514

VALIDATION:
- jobDescription: string, max 5000 chars
- tone: must be one of the 3 options
- Rate limit by IP
```

### File 2: Page

```
Create src/app/dashboard/cover-letter/page.tsx:

A page to generate cover letters.
- "use client"
- Large textarea for job description
- Tone selector: 3 radio buttons or toggle
  (Formal / Conversational / Technical)
- "Generate" button
- Loading state
- Results:
  - Cover letter displayed in a readable format
  - Key points highlighted as tags
  - Copy button for the letter
  - "Regenerate" button to try different tone
- Dark theme, responsive
- Back link to /dashboard
```

## 4.3 Test It

1. Paste a job description
2. Generate with each tone — do they actually sound different?
3. Check: Does the letter reference YOUR specific experience?
4. Check: Does it mention the job's specific requirements?

| Tone | Should Sound Like |
|------|------------------|
| Formal | "I am confident that my experience..." |
| Conversational | "What excites me about this role..." |
| Technical | "My implementation of X reduced latency by Y..." |

## Checklist

- [ ] `src/app/api/cover-letter/route.ts` — Claude-powered generation
- [ ] `src/app/dashboard/cover-letter/page.tsx` — paste + tone + results
- [ ] All 3 tones produce noticeably different output
- [ ] Letter references specific experience (not generic)
