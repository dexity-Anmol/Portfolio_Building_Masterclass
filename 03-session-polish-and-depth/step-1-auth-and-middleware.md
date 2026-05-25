# Step 1: Authentication & Middleware

> Protect your private tools with a simple password gate. ~20 minutes.

## 1.1 The Auth Model

```
Public (no auth)           Protected (password required)
─────────────────          ─────────────────────────────
/                          /dashboard
/api/chat                  /dashboard/fit-analyzer
                           /dashboard/resume-tailor
                           /dashboard/cover-letter
                           /api/fit-analyzer
                           /api/resume-tailor
                           /api/cover-letter
```

**How it works:**
1. User visits `/dashboard` → middleware checks for auth cookie
2. No cookie? → redirect to `/login`
3. Login page → user enters password → POST to `/api/auth`
4. Correct password → set httpOnly cookie → redirect to `/dashboard`
5. All `/dashboard/*` routes are now accessible

## 1.2 Add the Password

In `.env.local`, add:

```
SITE_PASSWORD=your-chosen-password
```

## 1.3 Build the 4 Files

Build in this order:

### File 1: Middleware

```
Create src/middleware.ts:

Next.js middleware that protects /dashboard routes.

BEHAVIOR:
- Check for cookie named "auth-token"
- If cookie value matches a hash of SITE_PASSWORD, allow
- If missing or wrong, redirect to /login
- Apply to paths: /dashboard, /dashboard/*, /api/fit-analyzer,
  /api/resume-tailor, /api/cover-letter
- Do NOT protect: /, /login, /api/chat, /api/auth

Use Next.js middleware matcher config.
Export config with matcher array.
```

### File 2: Auth API Route

```
Create src/app/api/auth/route.ts:

POST handler for login/logout.

LOGIN (POST with { password }):
- Compare password against process.env.SITE_PASSWORD
- If match: set httpOnly cookie "auth-token" with hashed value,
  return { success: true }
- If wrong: return 401 { error: "Invalid password" }
- Cookie settings: httpOnly, secure, sameSite: 'strict',
  path: '/', maxAge: 24 hours

LOGOUT (DELETE):
- Clear the auth-token cookie
- Return { success: true }
```

### File 3: Login Page

```
Create src/app/login/page.tsx:

A simple login page.
- "use client"
- Password input field + Submit button
- POST to /api/auth with { password }
- On success: redirect to /dashboard
- On failure: show error message
- Dark theme matching the site
- Centered on page, card-style container
```

### File 4: Dashboard Layout

```
Create src/app/dashboard/layout.tsx:

"use client" layout wrapper for all dashboard pages.
- Tab navigation: Overview, Fit Analyzer, Resume Tailor, Cover Letter
- Active tab highlighted in blue (#3b82f6)
- Logout button that calls DELETE /api/auth then redirects to /
- Dark theme, padded container
- Wraps {children}
```

### File 5: Dashboard Hub

```
Create src/app/dashboard/page.tsx:

A landing page for private tools.
- "use client"
- Cards linking to: /dashboard/fit-analyzer,
  /dashboard/resume-tailor, /dashboard/cover-letter
- Each card: icon, title, 1-line description
- Logout button (DELETE to /api/auth, redirect to /)
- Dark theme, responsive grid
```

## 1.4 Test the Auth Flow

1. Visit `localhost:3000/dashboard` → should redirect to `/login`
2. Enter wrong password → should show error
3. Enter correct password → should land on dashboard
4. Visit `/dashboard/fit-analyzer` → should work (cookie set)
5. Click logout → should redirect to home
6. Visit `/dashboard` again → should redirect to `/login`

## Checklist

- [ ] `SITE_PASSWORD` in `.env.local`
- [ ] `src/middleware.ts` — protects `/dashboard/*` routes
- [ ] `src/app/api/auth/route.ts` — login + logout
- [ ] `src/app/login/page.tsx` — password form
- [ ] `src/app/dashboard/layout.tsx` — tab navigation + logout
- [ ] `src/app/dashboard/page.tsx` — tool hub
- [ ] Full auth flow tested
