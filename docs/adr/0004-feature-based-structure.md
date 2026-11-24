# 0004. Feature-based folder structure

**Status:** Accepted (2026-11-24)

## Context

The app has several distinct areas (campaigns, characters, parties, enemies, dice, combat) that will each grow their own UI, business rules and database queries. The default Next.js layout groups code by type (`components/`, `lib/`), which turns into a few huge folders as features are added.

I come from Flutter, where I organised projects by feature with `presentation`, `domain` and `data` layers. I wanted that separation here without bringing over all of Clean Architecture.

## Decision

Group code by feature, with light layers inside each one:

```
src/
├── app/                  # routes only, pages stay thin
├── features/
│   └── campaigns/
│       ├── components/   # UI (presentation)
│       ├── actions.ts    # server actions, entry point for writes
│       ├── domain.ts     # business rules and Zod schemas (domain)
│       └── queries.ts    # Drizzle reads/writes (data)
├── db/                   # shared Drizzle schema
└── lib/                  # shared helpers
```

Rules:

- Pages in `app/` call into `features/` and hold no business logic.
- UI used by a single page can live next to it in `app/.../_components/`.
- Domain code is plain TypeScript with no database or React imports, so it can be unit tested on its own.
- Features don't import each other's internals. If `combat` needs character data, it calls `characters/queries.ts`, never character components. This will be enforced with ESLint.

## Consequences

- Everything about a feature is in one folder, and every feature has the same shape.
- Pure logic like the dice parser and initiative ordering is easy to test.
- Plain functions instead of repository interfaces and use-case classes, so less boilerplate than the Flutter version.
- Cross-feature imports need care until the ESLint rule is in place.

## Alternatives considered

- **Type-based (Next.js default):** simplest, but doesn't scale past a few features.
- **Everything colocated in `app/`:** good for page-specific code, but shared logic has no natural home.
- **Feature-Sliced Design:** strict and well documented, but heavy for a solo project, and its `app`/`pages` layers clash with Next.js's own folders.
- **Clean / Hexagonal Architecture:** lots of interfaces and adapters, and it works against Server Components, which already mix data fetching and UI.

## When to revisit

If a second client is added (e.g. a mobile app), move the schema and shared logic into packages in a monorepo.
