# Session 2: Build & Talk (2 hours)

> From structured content to a live, running portfolio with an AI chatbot.

## What You'll Build This Session

```
my-portfolio/
├── src/
│   ├── app/
│   │   ├── layout.tsx           ← Dark theme shell
│   │   ├── page.tsx             ← Homepage (6 sections)
│   │   └── api/chat/route.ts    ← AMA chatbot API
│   ├── components/
│   │   ├── Hero.tsx
│   │   ├── Experience.tsx
│   │   ├── Skills.tsx
│   │   ├── Projects.tsx
│   │   ├── Education.tsx
│   │   ├── Contact.tsx
│   │   └── ChatAssistant.tsx    ← Floating chat widget
│   └── lib/
│       └── rate-limiter.ts      ← API abuse protection
├── data/
│   ├── content.json             ← From Session 1
│   └── system-prompt.txt        ← From Session 1
└── .env.local                   ← ANTHROPIC_API_KEY
```

## Session Flow

| Time | Step | Guide | Outcome |
|------|------|-------|---------|
| 15 min | Scaffold Next.js Project | [step-1-project-scaffold.md](step-1-project-scaffold.md) | Running dev server |
| 30 min | Build 6 Homepage Sections | [step-2-core-pages.md](step-2-core-pages.md) | Dark-themed portfolio |
| 25 min | Build AMA Chatbot | [step-3-chatbot-build.md](step-3-chatbot-build.md) | Working chat widget |
| 20 min | Adversarial Testing | [step-4-adversarial-testing.md](step-4-adversarial-testing.md) | Hardened chatbot |
| 20 min | Build & Debug | [step-5-build-and-debug.md](step-5-build-and-debug.md) | Clean build |
| 10 min | Preview Session 3 | — | Motivation |

## Key Skills Practiced

- **Prompt-to-file workflow** — generating components from constrained prompts
- **Build order** — understanding why file order matters
- **Component patterns** — prompting for reusable UI components
- **Iterative fixing** — reading errors, writing targeted fix prompts
- **Adversarial testing** — trying to break your own chatbot

## Prerequisites

- Session 1 complete: `data/content.json` + `data/system-prompt.txt`
- Node.js 18+ installed
- Claude API key from [console.anthropic.com](https://console.anthropic.com)

## Copy-Paste Prompts

All prompts for this session: [prompts.md](prompts.md)
