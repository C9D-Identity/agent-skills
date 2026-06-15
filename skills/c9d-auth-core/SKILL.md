---
name: c9d-auth-core
description: Use when implementing framework-agnostic server auth primitives with C9D Identity (env contract, OIDC discovery, PKCE helpers, token exchange payloads, and signed session payloads).
---

# C9D Identity — @c9d/auth-core (server primitives)

## When to use this skill

- Building shared server authentication utilities not tied to one framework.
- Implementing OIDC/PKCE flows in backend runtimes.
- Standardizing C9D env contract validation and secure session payload handling.

Use `c9d-auth-next` for Next.js route/middleware adapters and `c9d-auth-react` for browser UI.

## Environment contract

Required server vars:

- `C9D_IDENTITY_URL`
- `C9D_OAUTH_CLIENT_ID`
- `C9D_SESSION_SECRET`
- `APP_BASE_URL` (or `NEXT_PUBLIC_APP_URL`)

Public fallbacks:

- `NEXT_PUBLIC_C9D_IDENTITY_URL`
- `NEXT_PUBLIC_C9D_OAUTH_CLIENT_ID`

Optional confidential exchange:

- `C9D_OAUTH_CLIENT_SECRET` (server-only)

Never expose a `NEXT_PUBLIC_C9D_OAUTH_CLIENT_SECRET`.

Generate `C9D_SESSION_SECRET`:

```bash
openssl rand -base64 32
```

## Capabilities in @c9d/auth-core

- Runtime env resolvers and error formatting
- OIDC discovery fetch/cache helper
- PKCE/state/session helper primitives
- Signed payload encode/verify helpers
- Token body builder that appends `client_secret` only when configured

## Hosted documentation

- SDK docs hub: `{identity}/docs/sdk`
- Auth core guide: `{identity}/docs/sdk/auth-core`
- Downloadable copy of this skill: `{identity}/resources/cursor/c9d-auth-core/SKILL.md`
