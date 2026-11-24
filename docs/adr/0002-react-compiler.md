# 0002. React Compiler on, Cache Components off for now

**Status:** Accepted (2026-11-24)

## Context

Next.js 16 supports two optional features:

- **React Compiler** memoizes components automatically, so there's no need for manual `useMemo` / `useCallback`.
- **Cache Components** makes caching opt-in with `"use cache"`, and requires anything reading cookies or request data to sit inside `<Suspense>`.

## Decision

Turn on the React Compiler. Leave Cache Components off.

Almost all data in this app belongs to a specific user (their campaigns, characters, encounters), so there isn't much to cache yet. Auth also reads cookies on most pages, which would mean dealing with Suspense boundaries everywhere before the basics work.

## Consequences

- Simpler components without manual memoization. Builds are a bit slower.
- Revisit Cache Components once auth and the main features are done.
