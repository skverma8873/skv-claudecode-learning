# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Pocket Heist** — a Next.js 16 app. Starter project for the Claude Code Masterclass.

## Key Technologies

| Technology | Version | Purpose |
|---|---|---|
| Next.js | 16 | React framework with App Router |
| React | 19 | UI library |
| TypeScript | 5 | Type safety |
| Tailwind CSS | 4 | Utility-first styling |
| Vitest | 4 | Unit testing |
| Testing Library | 16 | Component testing utilities |
| ESLint | 9 | Linting (flat config format) |
| Lucide React | 0.556 | Icon library |

## Commands

```bash
npm install        # install dependencies before first run
npm run dev        # start dev server at http://localhost:3000
npm run build      # production build
npm start          # start production server (after build)
npm run lint       # run ESLint
npm run test       # run all tests (Vitest)
npx vitest run tests/components/Navbar.test.tsx  # run a single test file
```

## Architecture

### Routing

The app uses the Next.js App Router with two route groups:

- `app/(public)/` — unauthenticated pages (no Navbar):
  - `/` — splash page (root `page.tsx`); intended to redirect to `/heists` when logged in, `/login` when not — auth routing not yet implemented
  - `/login` — login form
  - `/signup` — signup form
  - `/preview` — UI component preview gallery
- `app/(dashboard)/` — authenticated pages behind a layout that includes the `Navbar`:
  - `/heists` — heist list (active, assigned, expired)
  - `/heists/create` — create heist form
  - `/heists/[id]` — dynamic heist detail page

### Styling Architecture

Styling is layered across three levels, applied in order from broadest to most specific:

**1. Global Theme (`app/globals.css` — `@theme` block)**

Design tokens defined as CSS custom properties via Tailwind v4's `@theme`. These become Tailwind utility classes automatically (e.g., `--color-primary` → `bg-primary`, `text-primary`).

| Token | Value | Usage |
|---|---|---|
| `--color-primary` | `#C27AFF` | Purple — primary accent |
| `--color-secondary` | `#FB64B6` | Pink — secondary accent |
| `--color-dark` | `#030712` | Main page background |
| `--color-light` | `#0A101D` | Navbar / card backgrounds |
| `--color-lighter` | `#101828` | Slightly lighter surface |
| `--color-success` | `#05DF72` | Success states |
| `--color-error` | `#FF6467` | Error states |
| `--color-heading` | `white` | Headings |
| `--color-body` | `#99A1AF` | Body text |
| `--font-sans` | `'Inter'` | Body font (Google Fonts) |

Base typography is also set here: `body` applies `font-sans text-body bg-dark`; headings apply `text-heading`.

**2. Global Utility Classes (`app/globals.css` — below `@theme`)**

Reusable layout and layout-helper classes for use directly in JSX:

- `.page-content` — centered content area (`my-4 mx-auto w-6xl min-w-2xl max-w-full`)
- `.center-content` — vertically centered flex column (`flex flex-col justify-center text-justify min-h-lvh`)
- `.form-title` — centered bold heading for forms (`text-center text-xl font-bold`)
- `.public h1` — scoped inside the public layout's `<main className="public">`, enlarges h1 to `text-4xl`

**3. Component-Scoped Styles (CSS Modules)**

Each component has a `ComponentName.module.css` file. To use Tailwind's `@apply` with custom theme tokens inside a CSS Module, the file must include:

```css
@reference "../../app/globals.css";
```

This gives the module access to the theme tokens without duplicating styles into the output. Without this line, `@apply bg-light` would fail because the custom token is unknown to the module's scope.

Example from `Navbar.module.css`:
```css
@reference "../../app/globals.css";

.siteNav {
  @apply bg-light px-2 py-4;
}
```

Components apply module classes via the `styles` import. Use Tailwind utility classes inline in JSX for one-off styling needs rather than creating a CSS Module class:
```tsx
import styles from "./Navbar.module.css"

// Module class for reusable/complex styles:
<div className={styles.siteNav}>

// Inline Tailwind for one-off needs:
<p className="text-sm mt-2 text-primary">...</p>
```

## Components

Shared UI components live in `components/` and are imported via the `@/` path alias (maps to the repo root, configured in `tsconfig.json`).

**Each component follows a 3-file pattern:**
```
components/
└── ComponentName/
    ├── ComponentName.tsx         # Component implementation
    ├── ComponentName.module.css  # Scoped CSS module (@reference globals.css at top)
    └── index.ts                  # Barrel export
```

The `index.ts` barrel re-exports the default so consumers import from the folder, not the file:
```ts
// index.ts
export { default } from "./ComponentName"

// consumer
import Navbar from "@/components/Navbar"   // resolves to index.ts → Navbar.tsx
```

**Current components:**
- `Navbar` — site nav with logo, tagline, and links to `/heists` and `/heists/create`

## Naming Conventions

- **Components:** PascalCase (`Navbar`, `HeistCard`)
- **Pages:** `page.tsx` (Next.js convention)
- **CSS Module classes:** camelCase (`siteNav`, `navLink`)
- **Global utility classes:** kebab-case (`page-content`, `center-content`)
- **Files/folders:** match component name exactly for component folders

## Testing

Tests live in `tests/` and mirror the `components/` structure:
```
tests/
└── components/
    └── Navbar.test.tsx   # mirrors components/Navbar/
```

- **Framework:** Vitest with jsdom environment
- **Utilities:** `@testing-library/react` + `@testing-library/jest-dom` matchers (configured in `vitest.setup.ts`)
- **Naming:** `ComponentName.test.tsx`
- **Run single file:** `npx vitest run tests/components/ComponentName.test.tsx`
