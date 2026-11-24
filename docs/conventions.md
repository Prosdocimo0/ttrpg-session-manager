# Code conventions

How code in this repo is named and organised. For branches, commits and pull requests, see [CONTRIBUTING.md](../CONTRIBUTING.md).

## Folder structure

Code is organised by feature ([ADR 0004](adr/0004-feature-based-structure.md)):

```
src/
├── app/                  # routes only, pages stay thin
├── features/
│   └── <feature>/
│       ├── components/   # UI
│       ├── actions.ts    # server actions (writes)
│       ├── domain.ts     # business rules and Zod schemas
│       └── queries.ts    # database reads/writes
├── db/                   # Drizzle schema
└── lib/                  # shared helpers
```

- A feature doesn't import another feature's components. Shared data goes through that feature's `queries.ts`.
- UI used by only one page can live next to it in `app/.../_components/`.
- `domain.ts` has no React or database imports, so it can be tested on its own.

## Naming

| What | Convention | Example |
|---|---|---|
| Folders | kebab-case | `features/character-sheets/` |
| Component files | kebab-case | `dice-roller.tsx` |
| Other files | kebab-case | `roll-dice.ts`, `queries.ts` |
| Test files | same name + `.test` | `roll-dice.test.ts` |
| React components | PascalCase | `DiceRoller` |
| Functions and variables | camelCase | `rollDice`, `currentTurn` |
| Hooks | camelCase starting with `use` | `useInitiative` |
| Types and interfaces | PascalCase | `Character`, `RollResult` |
| Zod schemas | camelCase ending in `Schema` | `characterSchema` |
| Constants | UPPER_SNAKE_CASE | `MAX_PARTY_SIZE` |
| Booleans | start with `is`, `has`, `can`, `should` | `isDead`, `canEdit` |
| Server actions | verb first | `createCampaign`, `updateHp` |
| Database tables and columns | snake_case | `campaign_members`, `created_at` |
| Environment variables | UPPER_SNAKE_CASE | `DATABASE_URL` |

Names in code are in English, including game terms (`hitPoints`, not `pontosDeVida`). UI text can be translated later.

## TypeScript

- No `any`. If a type is really unknown, use `unknown` and narrow it.
- Let Drizzle and Zod infer types instead of writing them twice (`typeof table.$inferSelect`, `z.infer<typeof schema>`).
- Prefer `type` for object shapes. Use `interface` only when something needs to be extended.
- Use named exports. Default exports only where Next.js requires them (`page.tsx`, `layout.tsx`).

## React and Next.js

- Components are Server Components by default. Add `"use client"` only when a component needs state, effects or browser events, and keep those components small.
- Fetch data in Server Components, not in `useEffect`.
- Every server action validates its input with Zod and checks that the user is allowed to do it.
- No manual `useMemo` / `useCallback`. The React Compiler handles memoisation ([ADR 0002](adr/0002-react-compiler.md)).

## General

- Small functions that do one thing, with names that say what they do.
- Comments explain *why*, not *what*. If code needs a comment to explain what it does, try renaming things first.
- No commented-out code. Git keeps the history.
- Never commit secrets. Use `.env.local`, and add new variables to `.env.example` without values.
