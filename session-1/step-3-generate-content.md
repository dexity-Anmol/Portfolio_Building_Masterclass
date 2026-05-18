# Step 3: Generate content.json + Personal Touches

> Use constrained prompting to generate your structured content. ~25 minutes.

## 3.1 The RCCFA Formula

Every generation prompt needs 5 parts:

| Part | Purpose | If You Skip It |
|------|---------|---------------|
| **ROLE** | Sets the AI's perspective | Generic chatbot voice |
| **CONTEXT** | Gives raw material | AI invents everything |
| **CONSTRAINTS** | Rules output must obey | Vague, unmeasurable output |
| **FORMAT** | Exact shape of output | Wrong structure |
| **ANTI** | Explicit prohibitions | AI's worst habits appear |

## 3.2 Your First Real Prompt

Open Copilot Chat (`Ctrl+Shift+I`). Paste this, filling in YOUR details:

```
I'm building a portfolio site. Generate a data/content.json file
from my LinkedIn profile content below.

ROLE: You are a professional resume writer who values specificity.

CONTEXT: [Paste your LinkedIn PDF text or key bullets here]

CONSTRAINTS:
- hero.pitch: exactly 4 sentences, must contain 2+ numbers
- hero.profileImage: "/profile.jpg"
- meta.tagline: ≤ 8 words, memorable, specific to ME
- Every experience bullet starts with an action verb and includes a metric
- skills.gaps must have at least 3 items I'm genuinely weak in
- skills.strong has max 10 items
- No buzzwords: "passionate", "innovative", "proven track record"
- projects array has at least 3 entries
- Each project has "impact" with a quantified outcome
[ADD YOUR 3 CUSTOM RULES FROM EXERCISE 1 HERE]

FORMAT: Valid JSON matching this schema:
{ hero: {...}, experience: [...], skills: {...}, projects: [...],
  education: [...], meta: { siteTitle, siteDescription, tagline } }

ANTI-CONSTRAINTS:
- Do NOT invent metrics I didn't provide (estimate with ~ if needed)
- Do NOT include skills I haven't used professionally
- Do NOT write more than 3 bullets per job
- Do NOT add emoji or markdown inside JSON values
```

## Exercise 2: Generate Your content.json (~10 min)

1. Copy your LinkedIn content into the CONTEXT section
2. Add your 3 custom rules to CONSTRAINTS
3. Send it in Copilot Chat
4. Save the output:
   ```powershell
   mkdir data
   # Right-click in VS Code Explorer → New File → data/content.json
   # Paste the generated JSON
   ```
5. Verify it's valid JSON (no red squiggles in VS Code)

## 3.3 Add Your Profile Picture

**Options:** Professional headshot, casual but clear photo, or illustrated avatar.
**Requirements:** Square-ish, at least 400x400px, JPG or PNG.

## Exercise 3: Add Profile Picture (~5 min)

1. Save your photo as `public/profile.jpg`:
   ```powershell
   mkdir public
   # Drag your photo into public/ folder, rename to profile.jpg
   ```
2. Verify `hero.profileImage` in content.json is `"/profile.jpg"`

## 3.4 Write Your Tagline

The `meta.tagline` field — **8 words or fewer** that appear under your name.

| ❌ Generic | ✅ Memorable |
|-----------|-------------|
| "Full-stack developer" | "I make ML models behave in production" |
| "Passionate about technology" | "Security engineer who breaks things to fix them" |
| "Building the future" | "Teaching machines to read medical images" |

## Exercise 4: Write Your Tagline (~5 min)

Write your tagline. Put it in `data/content.json` under `meta.tagline`.
**Test:** Would your neighbor remember it tomorrow?

## Checklist

- [ ] `data/content.json` generated and saved
- [ ] Valid JSON (no red squiggles)
- [ ] `public/profile.jpg` added
- [ ] `meta.tagline` written (≤8 words, memorable)
