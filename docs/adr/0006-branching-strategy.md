# 0006. Branching with main and develop

**Status:** Accepted (2026-11-24)

## Context

I want changes to reach production in batches instead of one by one, with a place to test and undo my own mistakes before they go live. The project may also become open source, so contributors need a clear branch to target.

## Decision

Two long-lived branches:

- **`main`**: production. Only receives releases from `develop` (and urgent hotfixes). Every merge is tagged with a version.
- **`develop`**: integration. All feature work lands here first. This is where things get tested together and reverted if needed.

Short-lived branches for the actual work, always created from `develop`:

```
feat/dice-roller ──┐
fix/hp-overflow ───┼──► develop ──(every 1-2 weeks)──► main ──► v0.2.0
docs/setup ────────┘
```

| Change | Branch from | Pull request into | Merge method |
|---|---|---|---|
| Feature, fix, docs, chore | `develop` | `develop` | Squash merge |
| Release | `develop` | `main` | Merge commit, then tag `vX.Y.Z` |
| Hotfix (urgent production bug) | `main` | `main`, then merge `main` back into `develop` | Squash merge |

Releases use a merge commit, not a squash, so `develop` and `main` keep a shared history and don't drift apart.

Both `main` and `develop` are protected: pull request required, CI must pass, no force pushes, no direct pushes.

Environments:

| Environment | Source |
|---|---|
| Local | `pnpm dev` on any branch |
| Preview | Vercel preview deployment per pull request |
| Development | `develop` branch, deployed by Vercel |
| Production | `main` branch, deployed by Vercel |

## Consequences

- Production only changes on release days, with a version number and changelog for each release.
- Mistakes can be caught and reverted on `develop` without affecting users.
- Every change is merged twice (into `develop`, then in a release to `main`), which is more work than a single-branch flow.
- Hotfixes need an extra step: merging `main` back into `develop` so the fix isn't lost.
- Contributors must target `develop`. `develop` is set as the default branch on GitHub so new pull requests point there automatically.

## Alternatives considered

- **GitHub Flow** (only `main` + feature branches): simpler, but every merge goes straight to production. I want batched releases and a buffer for my own mistakes.
- **A separate `staging` branch**: `develop` already runs CI and has its own deployment, so a third branch would only add merging.
- **Full Git Flow** (`release/*` branches): release branches are useful for stabilising a big release over days. Here releases go straight from `develop` to `main`.
