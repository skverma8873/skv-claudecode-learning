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

## Components

Shared UI components live in `components/` and are imported via the `@/` path alias (maps to the repo root, configured in `tsconfig.json`).

**Each component follows a 3-file pattern:**
```
components/
└── ComponentName/
    ├── ComponentName.tsx      # Component implementation
    ├── ComponentName.module.css  # Scoped CSS module
    └── index.ts               # Barrel export: export { default } from "./ComponentName"
```

Import example: `import Navbar from "@/components/Navbar"`

**Current components:**
- `Navbar` — site nav with logo, tagline, and links to `/heists` and `/heists/create`

## Design System

Global styles and theme variables are defined in `app/globals.css` using Tailwind v4's `@theme` block.

**Color tokens:**
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

**Global utility classes (defined in `globals.css`):**
- `.page-content` — centered content area with max-width (`w-6xl`, `min-w-2xl`)
- `.center-content` — vertically centered flex column (`min-h-lvh`)
- `.form-title` — centered, bold, `text-xl` heading for forms
- `.public` on `<main>` — wraps unauthenticated pages; applies `text-4xl` to `h1`

**CSS approach:** Tailwind v4 utility classes + CSS Modules for scoped component styles. Avoid plain CSS class names on global elements; use CSS Modules for component-specific styling.

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
