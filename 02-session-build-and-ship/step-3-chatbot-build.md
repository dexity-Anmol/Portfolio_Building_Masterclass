# Step 3: Build the AMA Chatbot

> Rate limiter → API route → Chat widget. 3 files, strict order. ~25 minutes.

## 3.1 Why This Order?

```
1. src/lib/rate-limiter.ts     ← Utility (no dependencies)
2. src/app/api/chat/route.ts   ← API (depends on rate-limiter)
3. src/components/ChatAssistant.tsx  ← UI (depends on API)
```

Each file calls the one above it. Build bottom-up.

## 3.2 File 1: Rate Limiter

In Copilot Chat:

```
Create src/lib/rate-limiter.ts:

An in-memory rate limiter for API routes.
- Export a function that takes an IP string and returns
  { allowed: boolean, remaining: number }
- Window: 60 seconds
- Max requests per window: 20
- Use a Map<string, { count, resetTime }> for storage
- Clean up expired entries on each call
- TypeScript, no external dependencies
```

**Why rate limiting?** Without it, someone can send 10,000 requests/minute and drain your Claude API credits ($$$).

## 3.3 File 2: Chat API Route

```
Create src/app/api/chat/route.ts:

A Next.js App Router API route for the AMA chatbot.

BEHAVIOR:
- POST handler, accepts JSON { message: string }
- Read data/system-prompt.txt and data/content.json at startup
- Build system message: system prompt + JSON context
- Call Claude API (claude-sonnet-4-20250514, max_tokens: 300)
- Return JSON { reply: string }

SECURITY:
- Import and call rate limiter — reject with 429 if exceeded
- Validate message: must be string, max 500 characters
- Strip any HTML tags from user message before sending to Claude
- Return 400 for invalid input, 500 for API errors (generic message)
- Use environment variable ANTHROPIC_API_KEY

FORMAT: TypeScript, Next.js App Router route handler (export async function POST)
```

## 3.4 File 3: Chat Widget

```
Create src/components/ChatAssistant.tsx:

A floating chat widget in the bottom-right corner.

UI:
- Collapsed state: circular blue button with chat icon
- Expanded state: chat panel (400px wide, 500px tall)
- Message list with user/assistant message bubbles
- Input field + send button at bottom
- Dark theme matching the site (bg-[#1a1a2e], text-[#ededed])

BEHAVIOR:
- Toggle open/closed on button click
- Send message to /api/chat via POST
- Show typing indicator while waiting
- Display response in assistant bubble
- Clear input after send
- Enter key to send, Shift+Enter for newline
- Scroll to bottom on new messages

STATE:
- messages: { role: 'user' | 'assistant', content: string }[]
- isOpen: boolean
- isLoading: boolean
- input: string

TypeScript, "use client" directive, framer-motion for open/close animation.
```

## 3.5 Add ChatAssistant to Layout

Add the widget to `src/app/layout.tsx` so it appears on every page:

```
Update src/app/layout.tsx to import and render <ChatAssistant />
right before the closing </body> tag. Since ChatAssistant is a
client component, this works fine in the server layout.
```

## 3.6 Test the Chatbot

1. Make sure `npm run dev` is running
2. Click the chat button (bottom-right)
3. Ask: "What do you do?"
4. Should get a response based on your content.json

**Not working?** Check:
- `.env.local` has valid `ANTHROPIC_API_KEY`
- `data/content.json` and `data/system-prompt.txt` exist
- Terminal shows no import errors

## Checklist

- [ ] `src/lib/rate-limiter.ts` — in-memory rate limiter
- [ ] `src/app/api/chat/route.ts` — Claude API integration
- [ ] `src/components/ChatAssistant.tsx` — floating chat widget
- [ ] Widget added to `layout.tsx`
- [ ] Chatbot responds to questions
