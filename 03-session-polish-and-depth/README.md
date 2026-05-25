# Session 3: Your Private Tools & Going Live (2 hours)

> Authentication, AI-powered career tools, final polish, and deployment.

## What You'll Build This Session

```
my-portfolio/
├── src/
│   ├── app/
│   │   ├── login/page.tsx                ← Login page
│   │   ├── dashboard/
│   │   │   ├── page.tsx                  ← Tool hub
│   │   │   ├── fit-analyzer/page.tsx     ← Job fit scoring
│   │   │   ├── resume-tailor/page.tsx    ← Tailored resume
│   │   │   └── cover-letter/page.tsx     ← Cover letter generator
│   │   ├── api/
│   │   │   ├── auth/route.ts             ← Login/logout
│   │   │   ├── fit-analyzer/route.ts     ← AI analysis
│   │   │   ├── resume-tailor/route.ts    ← AI tailoring
│   │   │   └── cover-letter/route.ts     ← AI generation
│   │   └── middleware.ts                 ← Auth gate for /dashboard/*
│   └── ...
└── .env.local                            ← + SITE_PASSWORD
```

## Session Flow

| Time | Step | Guide | Outcome |
|------|------|-------|---------|
| 20 min | Auth & Middleware | [step-1-auth-and-middleware.md](step-1-auth-and-middleware.md) | Protected /dashboard |
| 15 min | Fit Analyzer | [step-2-fit-analyzer.md](step-2-fit-analyzer.md) | Job match scoring |
| 15 min | Resume Tailor | [step-3-resume-tailor.md](step-3-resume-tailor.md) | Tailored bullets |
| 10 min | Cover Letter | [step-4-cover-letter.md](step-4-cover-letter.md) | Tone-toggled letters |
| 15 min | Reskin & Security | [step-5-reskin-and-security.md](step-5-reskin-and-security.md) | Polished + audited |
| 25 min | Deployment | [step-6-deployment.md](step-6-deployment.md) | LIVE on the internet |
| 10 min | Peer Review + Wrap | — | Celebration |

## Key Skills Practiced

- **Auth patterns** — middleware, httpOnly cookies, route protection
- **Feature blueprints** — interface-first prompting for new tools
- **Multi-file generation** — API route + page + prompt in one shot
- **Security auditing** — 6-point checklist
- **Deployment** — git, GitHub, Vercel

## Prerequisites

- Session 2 complete: working portfolio with chatbot
- `npm run build` passes
- Git installed

## Copy-Paste Prompts

All prompts for this session: [prompts.md](prompts.md)
