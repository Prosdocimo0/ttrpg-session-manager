# 0003. Supabase and Drizzle for data and auth

**Status:** Proposed (2026-11-24)

## Context

The data is relational: campaigns have players, sessions, characters and encounters. The app also needs login, and the combat tracker should update live for every player at the table.

## Decision

- Supabase for Postgres, auth and realtime.
- Drizzle ORM to define the schema in TypeScript and generate migrations.
- Row Level Security so players can only see campaigns they're part of.

## Consequences

- One service covers database, auth and realtime, and the free tier is enough for this project.
- Queries are typed from the schema.
- Local development needs Docker for the Supabase CLI.
- Auth is tied to Supabase. Moving away later would mean redoing login.
- Firebase was an option, but a document database is a bad fit for this kind of relational data.
