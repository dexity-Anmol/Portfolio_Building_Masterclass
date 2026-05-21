# Step 2: Core Pages — Dark Theme + 6 Homepage Sections

> Build the dark-themed homepage with all 6 content sections. ~30 minutes.

## 2.1 Build Order (Why It Matters)

Build in this order — each file depends on the ones before it:

```
1. globals.css       ← Dark theme variables
2. Navigation.tsx    ← Fixed nav bar with section links
3. Footer.tsx        ← Simple footer
4. Hero.tsx          ← First section
5. Experience.tsx    ← Timeline
6. Skills.tsx        ← Categorized grid
7. Projects.tsx      ← Cards
8. Education.tsx     ← Simple list
9. Contact.tsx       ← CTA
10. layout.tsx       ← Shell (imports Nav, Footer, ChatAssistant)
11. page.tsx         ← Assembles all sections
```

**Why?** If you build `page.tsx` first, Copilot doesn't know what components exist. If you build components before the layout, they don't know what CSS variables are available.

## 2.2 Dark Theme Base

In Copilot Chat:

```
Replace src/app/globals.css with a dark theme:
- Background: #0a0a0a
- Text: #ededed
- Accent: a blue (#3b82f6) for links and highlights
- Card background: #1a1a2e
- Border: #2a2a3e
- Use CSS custom properties for all colors
- Include smooth scroll behavior
- Reset default margins
```

## 2.3 The Layout Shell

```
Replace src/app/layout.tsx with:
- Dark background using the CSS variables from globals.css
- Inter font from next/font/google
- Metadata: title from my data/content.json meta.siteTitle,
  description from meta.siteDescription
- Full viewport height, antialiased text
- Import and render Navigation above <main>
- Import and render Footer below <main>
- Import and render ChatAssistant
- Add pt-16 to <main> to offset the fixed nav bar
```

## 2.3b Navigation + Footer

```
Create src/components/Navigation.tsx:
- 'use client' (uses useState for mobile menu)
- Fixed position top of page, dark bg with transparency
  (bg-[#0a0a0f]/90 backdrop-blur-sm)
- Left: site name from content.json meta.siteTitle
- Right: nav links — About, Experience, Skills,
  Projects, Education, Contact
- Each link scrolls to corresponding section (href="#experience")
- MOBILE: hamburger icon toggles vertical menu
- z-50 to stay above other content
```

```
Create src/components/Footer.tsx:
- Simple footer at bottom of page
- Dark background, lighter border top
- Content: "© {year} {name}. Built with Next.js and Claude."
- Import name from content.json
- Server component (no 'use client')
```

## 2.4 Build Each Section Component

For each component, use this prompt pattern:

```
Create src/components/[Name].tsx:

- Read data from data/content.json ([which section])
- [Specific layout requirements]
- Dark theme: bg-[#0a0a0a], text-[#ededed], accent-[#3b82f6]
- Use framer-motion for fade-in on scroll
- TypeScript, functional component, default export
- Mobile responsive (single column on small screens)
```

### Component-Specific Details

| Component | Data Source | Key Layout |
|-----------|-----------|------------|
| **Hero** | `hero` | Large name, title, pitch, profile image, "Open to" badges |
| **Experience** | `experience[]` | Vertical timeline, company + title + dates, bullet list |
| **Skills** | `skills` | 3-column grid: Strong (blue), Moderate (gray), Gaps (orange) |
| **Projects** | `projects[]` | Cards with name, description, tech tags, impact |
| **Education** | `education[]` | Simple list: school, degree, year |
| **Contact** | `hero.openTo` | CTA section, email link or LinkedIn |

## 2.5 Assemble in page.tsx

```
Replace src/app/page.tsx:
- Import all 6 components: Hero, Experience, Skills, Projects,
  Education, Contact
- Render them in order, each in a <section> tag
- Full-width layout, sections stacked vertically
```

## 2.6 Verify

```powershell
npm run dev
```

Open localhost:3000. You should see:
- Dark background
- Your name, title, pitch, photo
- Timeline of jobs
- Skills in 3 categories
- Project cards
- Education
- Contact CTA

**Missing sections?** Check the browser console (F12) for import errors.

## Checklist

- [ ] `globals.css` — dark theme with CSS variables
- [ ] `layout.tsx` — dark shell with Inter font
- [ ] 6 section components created in `src/components/`
- [ ] `page.tsx` — assembles all 6 sections
- [ ] All sections visible at localhost:3000
- [ ] Mobile responsive (resize browser to check)
