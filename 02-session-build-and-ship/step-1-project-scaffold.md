# Step 1: Project Scaffold — Next.js Setup

> Create the project, install dependencies, configure environment. ~15 minutes.

## 1.1 Create the Next.js Project

In your terminal (Terminal → New Terminal in VS Code):

```powershell
npx create-next-app@latest my-portfolio --typescript --tailwind --eslint --app --src-dir --no-import-alias
cd my-portfolio
```

**If you already have a my-portfolio folder** from Session 1, run `create-next-app` in the parent directory — it will scaffold inside the existing folder.

## 1.2 Install Dependencies

```powershell
npm install @anthropic-ai/sdk framer-motion
```

| Package | What It Does |
|---------|-------------|
| `@anthropic-ai/sdk` | Claude API client for chatbot |
| `framer-motion` | Smooth scroll/fade animations |

## 1.3 Set Up Environment Variable

Create `.env.local` in the project root:

```
ANTHROPIC_API_KEY=sk-ant-your-key-here
```

Get your key from [console.anthropic.com](https://console.anthropic.com) → API Keys → Create Key.

**CRITICAL:** `.env.local` is in `.gitignore` by default. NEVER commit API keys.

## 1.4 Move Your Data Files

If your `data/` and `public/` files from Session 1 aren't already in the project:

```powershell
# Make sure these exist in your project root:
# data/content.json
# data/system-prompt.txt
# public/profile.jpg
```

## 1.5 Verify the Scaffold

```powershell
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) — you should see the default Next.js page.

## Expected File Tree

```
my-portfolio/
├── .env.local              ← API key (not committed)
├── package.json
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── data/
│   ├── content.json        ← From Session 1
│   └── system-prompt.txt   ← From Session 1
├── public/
│   └── profile.jpg         ← From Session 1
└── src/
    └── app/
        ├── layout.tsx
        ├── page.tsx
        └── globals.css
```

## Checklist

- [ ] `create-next-app` ran successfully
- [ ] `@anthropic-ai/sdk` and `framer-motion` installed
- [ ] `.env.local` has `ANTHROPIC_API_KEY`
- [ ] `data/content.json` and `data/system-prompt.txt` in project
- [ ] `public/profile.jpg` in project
- [ ] `npm run dev` shows default page at localhost:3000
