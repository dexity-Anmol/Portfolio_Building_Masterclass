# Step 2: Content Schema — LinkedIn Export & Data Design

> Gather raw content, then design the structure AI will fill. ~25 minutes.

## 2.1 Export Your LinkedIn Profile

1. Open linkedin.com → Your profile
2. Click **"More"** → **"Save to PDF"**
3. Save as `Profile.pdf` in your `my-portfolio/` folder

**No LinkedIn?** Use the [content worksheet](../00-pre-work/content-worksheet.md) instead. Write bullets for: current role, 3 past roles, top 10 skills, 3 projects.

## 2.2 What You Have (and What's Missing)

Your PDF gives you raw material:
- Job titles and dates ✓
- Company names ✓
- Bullet points (probably vague) ✓
- Skills list (probably too long) ✓

**What you DON'T have yet:**
- A structured format AI tools can consume
- Quantified metrics (numbers in every bullet)
- An honest assessment (strengths AND gaps)
- A concise pitch (not a 3-paragraph "about me")

## 2.3 What Is a Content Schema?

A **schema** is a contract: "My portfolio data looks like THIS."

```json
{
  "hero": { "name", "title", "company", "pitch", "openTo", "profileImage" },
  "experience": [
    { "company", "title", "start", "end", "bullets": ["verb + metric..."] }
  ],
  "skills": {
    "strong": ["skill1", ...],     // ≥8 items
    "moderate": ["skill1", ...],   // ≥3 items
    "gaps": ["skill1", ...]        // ≥3 items (honest!)
  },
  "projects": [
    { "name", "description", "technologies": [...], "impact": "..." }
  ],
  "education": [...],
  "meta": { "siteTitle", "siteDescription", "tagline" }
}
```

**Why this matters:** If you don't define the structure, AI will. And it'll guess wrong.

## 2.4 Key Schema Rules (Non-Negotiable)

| Field | Rule | Why |
|-------|------|-----|
| `hero.pitch` | ≤ 4 sentences, ≥ 2 numbers | Forces conciseness + proof |
| `experience[].bullets` | Each starts with verb, contains a number | Prevents filler |
| `skills.gaps` | ≥ 3 items | Forces honesty |
| `projects[].impact` | Quantified outcome | "Built X" is worthless without "that did Y" |
| `meta.tagline` | ≤ 8 words | Memorable one-liner |
| `hero.profileImage` | `"/profile.jpg"` | Personal touch |

## Exercise 1: Define YOUR Rules (~10 min)

Write **3 additional rules** for your content. Examples:
- "No bullet uses 'responsible for' or 'passionate about'"
- "Each experience entry has exactly 3 bullets"
- "skills.strong has no more than 10 items"

**Test:** Could someone verify your rule by reading your JSON?

## Checklist

- [ ] `Profile.pdf` saved in `my-portfolio/` (or worksheet filled out)
- [ ] Understand the schema structure
- [ ] 3 custom rules written down
