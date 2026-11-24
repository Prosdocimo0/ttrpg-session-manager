# Contributing

Thanks for your interest in the project! This guide covers how work moves through the repo. Code style and naming are in [docs/conventions.md](docs/conventions.md).

## Setup

```bash
git clone https://github.com/Prosdocimo0/ttrpg-session-manager.git
cd ttrpg-session-manager
pnpm install
pnpm dev
```

## Branches

The repo uses `main` (production) and `develop` (integration). See [ADR 0006](docs/adr/0006-branching-strategy.md) for the full reasoning.

- Always branch from `develop`, and open pull requests into `develop`.
- Never commit directly to `main` or `develop`.
- One branch per change. Keep it short-lived and delete it after merging.

Branch names use `<type>/<short-description>` in lowercase kebab-case:

```
feat/dice-roller
fix/initiative-order
docs/setup-instructions
refactor/character-form
chore/add-prettier
```

If the branch is for an issue, you can add the number: `feat/12-dice-roller`.

## Commits

Commits follow [Conventional Commits](https://www.conventionalcommits.org):

```
<type>(<optional scope>): <description>
```

| Type | Use for |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `refactor` | Code changes that don't change behaviour |
| `test` | Adding or fixing tests |
| `perf` | Performance improvements |
| `style` | Formatting only |
| `build` | Dependencies and build setup |
| `ci` | GitHub Actions and CI config |
| `chore` | Anything else (tooling, config) |

Good commit messages:

- Use the imperative: "add dice roller", not "added" or "adds".
- Lowercase, no period at the end, under about 72 characters.
- One logical change per commit. If the message needs "and", it might be two commits.
- Add a body after a blank line when the *why* isn't obvious.

```
feat(dice): support advantage and disadvantage rolls
fix(combat): keep turn order after removing a combatant
docs: add local Supabase setup to README
```

## Pull requests

A good pull request is small, focused and easy to review.

**Before opening one**

- Rebase on or merge the latest `develop`.
- Run `pnpm lint` and `pnpm build` locally.
- Review your own diff first. Remove debug logs and leftover comments.

**Title:** same format as a commit message, e.g. `feat(dice): add dice roller`. With squash merging, the title becomes the commit on `develop`.

**Description:** fill in the template. It asks for:

- **What** changed and **why**.
- **How to test** it.
- **Screenshots** or a short GIF for anything visual.
- The related issue (`Closes #12`), which closes the issue automatically on merge.

**Size:** aim for something reviewable in about 15 minutes. If a feature is big, split it into several pull requests (e.g. database schema first, then UI).

**While it's open**

- CI must pass before merging.
- Answer every review comment, either with a change or an explanation.
- Push fixes as new commits. They get squashed on merge anyway.

## Releases

Every one to two weeks, `develop` is merged into `main` through a pull request titled `release: vX.Y.Z`. That merge uses a merge commit (not squash) and gets a version tag.

Versions follow [Semantic Versioning](https://semver.org): while the project is below `1.0.0`, `feat` bumps the minor version (`0.2.0` → `0.3.0`) and `fix` bumps the patch (`0.2.0` → `0.2.1`).

## Game content

This app is built around Tormenta 20, whose rules text is copyrighted by Jambô Editora. Don't add text copied from official books (abilities, spells, monster stat blocks). Descriptions should be written in your own words or entered by users.
