# Step 5: Evaluate & Fix Content + Elevator Pitch

> Review your content with 5 checks, then validate it with a peer. ~25 minutes.

## 5.1 The 5 Checks

Open `data/content.json` and check EVERY section:

| # | Check | What to Look For |
|---|-------|-----------------|
| 1 | **Factual** | Did AI invent anything not in your source? |
| 2 | **Specific** | Numbers, company names, or generic phrases? |
| 3 | **Voice** | Sounds like you or like a chatbot? |
| 4 | **Structure** | Matches the schema exactly? |
| 5 | **Gaps** | What's MISSING that should be there? |

## 5.2 Red Flags vs. Good Signs

| ❌ Red Flags | ✅ Good Signs |
|-------------|-------------|
| "passionate about building scalable systems" | "Reduced review time by 90% across 750M+ devices" |
| "Led a team" (how many? what result?) | `skills.gaps: ["Rust", "Mobile dev", "WebGL"]` |
| `skills.gaps` is empty | Pitch has 4 sentences with real numbers |
| Generic tagline | `tagline: "Making ML models behave in prod"` |

## 5.3 How to Write Fix Prompts

**Formula:** [What's wrong] + [Where] + [What it should be]

❌ Bad: "Make the content better"

✅ Good:
```
In data/content.json, the second experience entry has a bullet
'Responsible for managing team projects.' Replace it with an
action-verb bullet that includes a metric. The team was 8 people
and we shipped 3 products in 6 months.
```

## Exercise 6: Find & Fix 3 Problems (~10 min)

1. Run through the 5 Checks
2. Pick the 3 worst problems
3. Write a targeted fix prompt for each
4. Apply via Copilot
5. Re-read — confirm only that part changed

| # of problems found | Assessment |
|---------------------|-----------|
| 0 | You're not looking hard enough |
| 1-2 | Good start, dig deeper |
| 3-5 | Realistic, well-reviewed |
| 6+ | Great eye — fix the worst 3, note the rest |

## 5.4 The Elevator Pitch

### Part 1: Read Your Pitch Out Loud (2 min)

Open `content.json`. Read your `hero.pitch` aloud to your neighbor.

**Does it sound natural?** If you stumble, rewrite until you can say it comfortably.

### Part 2: Peer Review (8 min)

Swap `data/content.json` files. Review theirs:

| Category | Question | Score |
|----------|----------|-------|
| **Accuracy** | Metrics plausible (not inflated)? | /5 |
| **Specificity** | Bullets name tools, numbers, outcomes? | /5 |
| **Persona** | Pitch sound like a human? | /5 |
| **Gaps honesty** | skills.gaps are real (not humble-brags)? | /5 |
| **Tagline** | Would you remember it tomorrow? | /5 |

**Write 2 specific improvements** → share with your partner.

## Checklist

- [ ] 5 Checks completed on content.json
- [ ] 3 problems found and fixed with targeted prompts
- [ ] Pitch read aloud — sounds natural
- [ ] Peer review completed — 2 improvements received
