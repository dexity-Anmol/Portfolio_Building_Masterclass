# Step 5: Build & Debug — Get a Clean Build

> Run the production build, read errors, fix them. ~20 minutes.

## 5.1 Why Build Before Deploying?

`npm run dev` is forgiving — it skips some checks. `npm run build` catches:
- Type errors (TypeScript)
- Missing imports
- Invalid data access
- SSR/client mismatches

**Rule: If it doesn't build, it doesn't deploy.**

## 5.2 Run the Build

```powershell
npm run build
```

### If It Passes

```
✓ Compiled successfully
✓ Collecting page data
✓ Generating static pages
```

Skip to 5.5 (Code Quality Check).

### If It Fails

You'll see errors. Read on.

## 5.3 How to Read Build Errors

Build errors have 3 parts:

```
Type error: Property 'title' does not exist on type '{ name: string; }'.
  > 12 | <h1>{data.title}</h1>
       |          ^^^^^
  at src/components/Hero.tsx:12:10
```

| Part | What It Tells You |
|------|------------------|
| **Error type** | `Type error` = TypeScript, `Module not found` = bad import |
| **Message** | What's wrong: missing property, wrong type, etc. |
| **Location** | `src/components/Hero.tsx:12:10` = file, line, column |

## 5.4 Fix Methodology

**Don't say:** "Fix all the build errors"

**Do say:**

```
Build error in src/components/Hero.tsx line 12:
"Property 'title' does not exist on type '{ name: string; }'."

My data/content.json has hero.title as a string.
The component reads content.json using:
import content from '@/../../data/content.json'

Fix ONLY this error. Show me the corrected lines.
```

### One Error at a Time

1. Read the FIRST error (later errors are often caused by the first)
2. Write a targeted fix prompt
3. Apply the fix
4. Run `npm run build` again
5. Repeat

### Common Build Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `Module not found: '@/components/X'` | Wrong import path | Check file name + path |
| `'X' is not a valid JSX element` | Missing "use client" | Add `"use client"` at top |
| `Cannot read properties of undefined` | content.json path wrong | Check import path to data/ |
| `Hydration mismatch` | Server/client render different | Move dynamic logic to useEffect |

## 5.5 Code Quality Check

Once the build passes, a quick sanity check:

```
Review my project for these issues:
1. Any hardcoded API keys or secrets in source files?
2. Any console.log statements that should be removed?
3. Any components missing "use client" that use hooks?
4. Is .env.local in .gitignore?

Check ONLY these 4 things. Report as a table: file, issue, fix.
```

## Checklist

- [ ] `npm run build` passes with no errors
- [ ] No hardcoded secrets in source files
- [ ] `.env.local` is in `.gitignore`
- [ ] No stray `console.log` statements
