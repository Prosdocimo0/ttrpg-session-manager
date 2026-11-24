# 0001. Next.js with TypeScript

**Status:** Accepted (2026-11-24)

## Context

The app needs an interactive UI (character sheets, combat tracker) and server code (auth, database). I'm building it alone, so I'd rather not maintain a separate frontend and backend.

## Decision

Next.js with the App Router, TypeScript in strict mode and Tailwind CSS.

## Consequences

- One codebase for UI and server logic. Server actions instead of a separate API.
- Server Components by default, client components only where there's interaction.
- Server vs. client components and caching rules take some learning.
- I considered React + Vite, but that would still need a backend.
