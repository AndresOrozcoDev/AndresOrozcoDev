---
name: frontend-dev-standards
description: >
  Apply personal frontend development standards when building, reviewing, scaffolding,
  or auditing any web application (HTML5, CSS3, JavaScript, React, Angular, or Tailwind CSS).
  Use this skill whenever the user asks to: create a new frontend project, generate a component,
  review code quality, set up project structure, create documentation templates, configure
  styling, add SEO or accessibility, set up environment variables, write tests, or prepare a
  deployment checklist. Also trigger when the user says "apply my standards", "follow my stack",
  "set up my project structure", or "use our conventions". This skill encodes the user's
  personal engineering methodology — always apply it even if the user does not explicitly ask.
---
 
# Frontend Dev Standards
 
This skill encodes the user's personal frontend engineering standards for HTML5, CSS3,
JavaScript (ES6+), React, and Angular projects. Apply all relevant sections based on the
technology in use. Never skip a section without a documented reason.
 
---
 
## 1. DOCUMENTATION
 
Every project ships with these files before the first real commit:
 
### README.md — Developer onboarding (setup in under 5 minutes)
Must include:
- Project name + one-line description
- Problem it solves
- Badges (build status, coverage, version, license)
- Tech stack list
- Requirements (Node version, package manager, env vars needed)
- Installation and execution commands (copy-paste ready)
- Test and deployment commands
- Link to official technology docs
- Author
### DOCS.md — Architecture reference
Must include:
- Annotated folder structure
- Architecture diagram (ASCII or linked image)
- Main data flow description
- API contracts consumed (method, endpoint, request/response shape)
- Environment variables table (key, description, example value)
- ER diagram if there is persistent data
- Deployment steps
### CHANGELOG.md — Version history
Format: Keep a Changelog (https://keepachangelog.com)
Sections per version: Added, Changed, Fixed, Removed
 
---
 
## 2. ARCHITECTURE AND STRUCTURE
 
### Folder structure (React example — mirror for Angular)
```
src/
├── features/        # Domain modules (auth, dashboard, profile)
│   └── auth/
│       ├── components/
│       ├── hooks/
│       ├── services/
│       ├── types/
│       └── tests/
├── pages/           # Route-level components only
├── components/      # Shared/reusable UI components
├── context/         # Global state providers
├── services/        # API calls and external integrations
├── utils/           # Pure helper functions
├── hooks/           # Shared custom hooks
├── types/           # Global TypeScript types and interfaces
└── tests/           # Integration tests (unit tests co-located)
```
 
### Rules
- Lazy loading on every route. No exceptions.
- Path aliases configured: `@/components`, `@/utils`, `@/services`, `@/types`
- Components only render — zero business logic inside JSX
- Business logic lives in services (API) and hooks (state + transformations)
- Angular: Standalone Components only. No NgModules unless forced by a legacy dependency.
---
 
## 3. CLEAN CODE
 
- **Single Responsibility**: every function does one thing. One line of JSDoc above it describing what it does.
- **Strict typing**: TypeScript Strict Mode always on. `any` is banned.
- **Comments**: comment the *why*, never the *what*. Self-documenting names.
- **No `console.log`** in code that reaches production. Use a logger utility or remove before commit.
- **Import order** (enforce with ESLint):
  1. External libraries
  2. Internal modules (`@/...`)
  3. Types
  - Blank line between each group.
### JSDoc example (one-liner per function)
```ts
/** Formats a UTC date string to DD/MM/YYYY for display */
const formatDate = (utc: string): string => { ... }
```
 
---
 
## 4. STYLES
 
- **Mobile-first**: all base styles target mobile; use `min-width` media queries to scale up.
- **CSS variables in `:root`**: define colors, typography scale, and spacing tokens here. Never hardcode.
- **Tailwind CSS** is the primary styling tool.
- **BEM** only for custom classes that cannot be expressed with Tailwind utilities.
- No `!important`. No styling by ID selector.
- `font-display: swap` on all custom fonts + `<link rel="preload">` for critical fonts.
- All `<img>` tags must have explicit `width` and `height` attributes to prevent layout shift (CLS).
### :root token example
```css
:root {
  --color-primary: #1a1a2e;
  --color-accent: #e94560;
  --font-size-base: 1rem;
  --spacing-md: 1.5rem;
}
```
 
---
 
## 5. SEO
 
Checklist per page/route:
- [ ] Unique `<title>` and `<meta name="description">` per page
- [ ] Open Graph tags: `og:title`, `og:description`, `og:image`, `og:url`
- [ ] Twitter Card tags: `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`
- [ ] Single `<h1>` per page; heading hierarchy H1 → H2 → H3 (no skipping)
- [ ] `sitemap.xml` at root
- [ ] `robots.txt` at root
- [ ] Images in WebP format, compressed, with descriptive filenames
---
 
## 6. ACCESSIBILITY
 
Non-negotiable baseline:
- `alt` on every `<img>`: descriptive text, or `alt=""` for purely decorative images
- `aria-label` on every interactive element with no visible text (icon buttons, icon-only links)
- `title` on `<iframe>` elements
- Color contrast ratio minimum 4.5:1 for normal text (verify with Chrome DevTools)
- Full keyboard navigation: Tab moves focus, Enter/Space activate, Escape closes modals
- Focus styles must be visible — never `outline: none` without a custom replacement
---
 
## 7. SECURITY
 
- Sanitize all user inputs before processing or rendering
- Sensitive values in `.env` — never hardcoded in source
- `.env` always in `.gitignore`
- `.env.example` committed with all required keys and no real values
- Server headers required:
  - `Content-Security-Policy`
  - `X-Frame-Options: DENY`
  - `X-Content-Type-Options: nosniff`
---
 
## 8. GLOBAL STATE
 
- Global store for data shared across routes: authenticated user, theme, locale
- Local `useState` for data used by a single component
- Rule of thumb: if only one component needs it → local state. If two or more → global.
- Context API for low-frequency global state (theme, user)
- Avoid storing derived data in the store — compute it at render time
---
 
## 9. TESTING
 
- Coverage target: >80%
- Test behavior, not implementation details. Query by role and label, not by class or internal state.
- Co-locate unit tests: `ComponentName.test.tsx` next to `ComponentName.tsx`
- Or group in `/tests` folder for integration-level tests
- One test file per component, service, or hook
- Structure: Arrange → Act → Assert
### What to test
- Components: renders correctly, user interactions produce expected output
- Services: correct data transformation, error handling
- Hooks: state changes on expected inputs
---
 
## 10. VERSION CONTROL
 
### Conventional Commits
Format: `type(scope): description`
 
| Type | When to use |
|------|------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no logic change |
| `refactor` | Code restructure, no behavior change |
| `test` | Adding or fixing tests |
| `chore` | Tooling, deps, config |
 
### Branch strategy
- `feature/descriptive-name` for new features
- `fix/descriptive-name` for bug fixes
- One logical change per commit — no bundling unrelated changes
- Merge to `main` only when the feature is complete and tested
---
 
## 11. PRE-DEPLOYMENT CHECKLIST
 
Run before every deploy:
 
```bash
# 1. Type check
npx tsc --noEmit
 
# 2. Lint
npm run lint
 
# 3. Tests
npm run test -- --coverage
 
# 4. Production build
npm run build
 
# 5. Dependency audit
npm audit --audit-level=moderate
```
 
Also verify manually:
- [ ] No `.env` production file committed to the repo
- [ ] No `console.log` statements in production code
- [ ] No hardcoded secrets or API keys in source
- [ ] All environment variables documented in `.env.example`
- [ ] CHANGELOG.md updated with this release
---
 
## Quick Reference: Tech-Specific Notes
 
### React
- Functional components only (no class components)
- Custom hooks for all stateful logic reused across components
- `React.memo` only with a measured performance reason — not by default
- `useEffect` with explicit dependency arrays — no empty array without a comment explaining why
### Angular
- Standalone Components — no `NgModule` unless unavoidable
- Signal-based state where available (Angular 17+)
- Services injectable at root unless scoped intentionally
- `OnPush` change detection strategy by default
### Static pages (HTML + CSS + JS only)
- Semantic HTML5 elements: `<header>`, `<main>`, `<section>`, `<article>`, `<footer>`, `<nav>`
- CSS classes only (no IDs for styling)
- ES6+: arrow functions, `const`/`let`, template literals, destructuring, modules
---
 
## When Generating Code
 
Always apply the relevant sections above. For every file generated:
1. TypeScript strict types — no shortcuts
2. One-line JSDoc per exported function
3. Imports in order: external → internal → types
4. No inline styles — use Tailwind classes or CSS variables
5. If it's a component, ask: does it have only one responsibility?
6. If it touches user input, sanitize it
7. If it's a new feature, remind the user to add a test file
 
