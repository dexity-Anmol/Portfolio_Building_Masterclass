# Session 2 Prompts — Copy & Paste Reference

> All prompts use Copilot Chat (`Ctrl+Shift+I`). Build files in order.

---

## Prompt 1: Dark Theme CSS

```
Replace src/app/globals.css with a dark theme:
- Background: #0a0a0a
- Text: #ededed
- Accent: #3b82f6 for links and highlights
- Card background: #1a1a2e
- Border: #2a2a3e
- Use CSS custom properties for all colors
- Include smooth scroll behavior
- Reset default margins
```

---

## Prompt 2: Layout Shell

```
Replace src/app/layout.tsx with:
- Dark background using CSS variables from globals.css
- Inter font from next/font/google
- Metadata: title "[Your Name] — Portfolio",
  description "[your tagline]"
- Full viewport height, antialiased text rendering
- Import globals.css
- Import and render Navigation above <main>
- Import and render Footer below <main>
- Import and render ChatAssistant
- Add pt-16 to <main> to offset the fixed nav bar
```

---

## Prompt 3: Section Component (repeat for each)

```
Create src/components/[Hero|Experience|Skills|Projects|Education|Contact].tsx:

- Read data from ../../data/content.json ([specify which field])
- [Specific layout: timeline / grid / cards / list]
- Dark theme: bg from CSS variable, text-[#ededed], accent-[#3b82f6]
- Use framer-motion for fade-in on scroll (useInView)
- TypeScript, "use client", functional component, default export
- Mobile responsive (stack on small screens)
- Section has id="[name]" for anchor links
```

---

## Prompt 4: Navigation Component

```
Create src/components/Navigation.tsx:
- 'use client' (uses useState for mobile menu)
- Fixed position, top of page, dark background with
  slight transparency (bg-[#0a0a0f]/90 backdrop-blur-sm)
- Left: site name from content.json meta.siteTitle
- Right: nav links — About, Experience, Skills,
  Projects, Education, Contact
- Each link scrolls to the corresponding section
  (href="#experience" etc.)
- MOBILE (below md:): hamburger icon toggles vertical menu
  - Clicking a link closes the menu
- z-50 to stay above other content
```

---

## Prompt 5: Footer Component

```
Create src/components/Footer.tsx:
- Simple footer at bottom of page
- Dark background, lighter border top
- Content: "© {year} {name}. Built with Next.js and Claude."
- Import name from content.json
- Server component (no 'use client')
```

---

## Prompt 6: Assemble Homepage

```
Replace src/app/page.tsx:
- Import Hero, Experience, Skills, Projects, Education, Contact
  from @/components/
- Render in order
- Full-width, vertical stack, no sidebar
```

---

## Prompt 7: Rate Limiter

```
Create src/lib/rate-limiter.ts:
- Export function checkRateLimit(ip: string):
  { allowed: boolean, remaining: number }
- In-memory Map: IP → { count, resetTime }
- Window: 60 seconds, max 20 requests
- Auto-cleanup expired entries
- TypeScript, no dependencies
```

---

## Prompt 8: Chat API Route

```
Create src/app/api/chat/route.ts:
- POST handler, accepts { message: string }
- Read data/system-prompt.txt + data/content.json
- System message = system prompt + "\n\nCONTEXT:\n" + JSON
- Call Claude API: claude-sonnet-4-20250514, max_tokens 300
- Return { reply: string }
- Rate limit by IP (import from @/lib/rate-limiter)
- Validate: message must be string, max 500 chars
- Strip HTML from input
- 400 for bad input, 429 for rate limit, 500 for API error
```

---

## Prompt 9: Chat Widget

```
Create src/components/ChatAssistant.tsx:
- "use client" component
- Floating button (bottom-right, blue circle, chat icon)
- Expands to 400x500px chat panel
- Message bubbles: user (right, blue) / assistant (left, gray)
- Input + send button, Enter to send
- POST to /api/chat, show typing indicator while loading
- Dark theme: bg-[#1a1a2e], text-[#ededed]
- framer-motion for open/close animation
- Auto-scroll to latest message
```

---

## Prompt 10: Fix a Build Error

```
Build error in [file] line [N]:
"[exact error message]"

The file does [brief context of what it does].
The data source is [where data comes from].

Fix ONLY this error. Show the corrected lines.
```

---

## Prompt 11: Code Quality Check

```
Review my project for:
1. Hardcoded API keys or secrets in source files?
2. console.log statements to remove?
3. Components missing "use client" that use hooks?
4. Is .env.local in .gitignore?

Report as a table: file, issue, fix.
```
