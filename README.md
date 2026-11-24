# TTRPG Session Manager

A web app for running tabletop RPG campaigns: sessions, character sheets, enemies, dice rolls and initiative tracking in one place.

I'm currently in a 3-year TTRPG campaign with my brother and some friends. After only a few sessions, we were already having a hard time using the same character sheet every weekend. Most players switched to physical notebooks just for RPG notes, NPC stories, money and items. But a notebook only has so many pages, and with a deep enough story, ours filled up fast.

When we tried other software, we ran into a new set of problems, and features our games really needed were still missing. Mostly, the tools were overcomplicated and unintuitive.

With my background in design and development, I decided to build my own solution for our table: something more interactive that makes things easier for both players and DMs. Something that speeds up character creation and decision-making during turns, and shows ability descriptions so players don't need to check the book to see what their abilities do.


**Status:** early development.

## Planned features

- [ ] Login and user profiles
- [ ] Campaigns and session notes
- [ ] Character sheets
- [ ] Party management
- [ ] Enemy bestiary for the GM
- [ ] Dice roller (`2d6+3` style)
- [ ] Initiative and combat tracker with live updates

## Stack

Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4, pnpm.

Planned: Supabase (Postgres, auth, realtime), Drizzle ORM, GitHub Actions, Vercel.

## Architecture

Code is organised by feature (`src/features/campaigns`, `src/features/combat`, ...), each with its own UI, business rules and database queries. There's no separate backend: Server Components read data and Server Actions write it. The reasoning is in [ADR 0004](docs/adr/0004-feature-based-structure.md) and [ADR 0005](docs/adr/0005-no-separate-api.md).

## Running locally

Requires Node.js 20.9+ and pnpm.

```bash
pnpm install
pnpm dev
```

Then open http://localhost:3000.

## Conventions

- Commits follow [Conventional Commits](https://www.conventionalcommits.org) (`feat:`, `fix:`, `docs:`, `chore:`...).
- Features are built on branches like `feat/dice-roller` and merged through pull requests.
- Bigger technical decisions are written down in [`docs/adr`](docs/adr).

## License

[MIT](LICENSE)
