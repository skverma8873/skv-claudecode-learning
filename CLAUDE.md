# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Pocket Heist** — a Next.js 16 app (React 19, TypeScript, Tailwind CSS v4). Starter project for the Claude Code Masterclass.

## Commands

```bash
npm install        # install dependencies
npm run dev        # start dev server at http://localhost:3000
npm run build      # production build
npm run lint       # run ESLint
npm run test       # run all tests (Vitest)
npx vitest run tests/components/Navbar.test.tsx  # run a single test file
```

## Architecture

The app uses the Next.js App Router with two route groups:

- `app/(public)/` — unauthenticated pages: landing, login, signup, preview
- `app/(dashboard)/` — authenticated pages behind a layout that includes the `Navbar`; contains heist list, detail (`[id]`), and create pages

Shared UI components live in `components/` and are imported via the `@/` path alias (configured in `tsconfig.json`).

Tests live in `tests/` and mirror the `components/` structure. Vitest runs in a jsdom environment with `@testing-library/react`.

## Project Structure

```
├── app/
│   ├── (dashboard)/
│   │   ├── heists/
│   │   │   ├── [id]/page.tsx       # heist detail page
│   │   │   ├── create/page.tsx     # create heist page
│   │   │   └── page.tsx            # heist list page
│   │   └── layout.tsx              # dashboard layout (includes Navbar)
│   ├── (public)/
│   │   ├── login/page.tsx
│   │   ├── preview/page.tsx
│   │   ├── signup/page.tsx
│   │   ├── page.tsx                # landing page
│   │   └── layout.tsx
│   ├── globals.css
│   └── layout.tsx                  # root layout
├── components/
│   └── Navbar/
│       ├── Navbar.tsx
│       ├── Navbar.module.css
│       └── index.ts
├── public/
│   └── skeleton.png
├── tests/
│   └── components/
│       └── Navbar.test.tsx
├── next.config.ts
├── tsconfig.json
├── vitest.config.mts
└── vitest.setup.ts
```
