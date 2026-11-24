# 0005. No separate backend API

**Status:** Accepted (2026-11-24)

## Context

The app needs to read and write data in Postgres. One option is a separate backend (e.g. a REST API) that the frontend calls. The other is to use Next.js's own server side.

## Decision

Keep the backend inside the Next.js app:

- **Reads:** Server Components call feature queries directly.
- **Writes:** forms call Server Actions, which validate input with Zod, check permissions and then run the query.
- **Route Handlers** (`app/api/.../route.ts`) only where something external needs an HTTP endpoint, like webhooks.

Database code only runs on the server. The browser never talks to Postgres directly.

## Consequences

- One project to build, deploy and type-check. Types flow from the Drizzle schema to the UI without duplication.
- No separate API layer to version or document.
- Every Server Action is effectively a public endpoint, so each one has to validate input and check auth itself. Row Level Security in Postgres is a second safety net.
- If another client needs the data later, an API will have to be added then.

## Alternatives considered

- **Separate REST API (e.g. Express or NestJS):** makes sense with multiple clients or separate teams. Here it would double the deployment and auth work for no benefit.
- **tRPC:** typed API calls, but Server Actions already give end-to-end types in Next.js.
- **Supabase client queries from the browser:** possible with RLS, but it spreads data access around the UI and makes business rules harder to keep in one place.
