---
name: c9d-auth-next
description: Use when integrating @c9d/auth-next into a product Next.js app for middleware/session/auth route handlers and embedded bridge primitives.
---

# C9D Identity — @c9d/auth-next (Next.js server SDK)

## When to use this skill

- Building or changing Next.js auth middleware / proxy behavior.
- Implementing OIDC login/callback/logout route handlers server-side.
- Standardizing server-side session cookie handling across product apps.
- Building SDK-first embedded bridge wrappers (`/api/auth/embedded/*`) for product-owned auth UX.

Use `c9d-auth-react` for client UI (`SignIn`, `SignUp`, hooks), `c9d-auth-core` for framework-neutral server primitives, and `c9d-identity-mcp` for admin automation.

## Before you start

1. Resolve identity base URL for target environment.
2. Obtain OAuth `clientId` for the product environment.
3. Confirm OAuth client redirect URI includes **product app callback**:
   - `https://<product-domain>/auth/callback`
   - Local: `http://localhost:3000/auth/callback`

## Required env contract

Server-first:

- `C9D_IDENTITY_URL`
- `C9D_OAUTH_CLIENT_ID`
- `C9D_SESSION_SECRET`

Public fallbacks:

- `NEXT_PUBLIC_C9D_IDENTITY_URL`
- `NEXT_PUBLIC_C9D_OAUTH_CLIENT_ID`

Optional confidential client:

- `C9D_OAUTH_CLIENT_SECRET` (server-only, never `NEXT_PUBLIC_*`)

Generate session secret:

```bash
openssl rand -base64 32
```

## Core usage

Install both SDK tarballs (manifest exposes exact URLs):

```bash
pnpm add https://identity.c9d.engineering/sdk/c9d-auth-core-{version}.tgz https://identity.c9d.engineering/sdk/c9d-auth-next-{version}.tgz
```

```ts
import { createC9DNextAuth } from "@c9d/auth-next";
export const c9dAuth = createC9DNextAuth();
```

- `c9dAuth.beginLogin(request)` for `/auth/login`
- `c9dAuth.handleCallback(request)` for `/auth/callback`
- `c9dAuth.handleLogout(request)` for `/auth/logout`
- `c9dAuth.getSession(request)` for server/middleware checks
- `c9dAuth.buildLoginRedirect(request)` for locale-aware unauthenticated redirects

Embedded bridge pattern:

```ts
import { createC9DEmbeddedBridge } from "@c9d/auth-next";

const bridge = createC9DEmbeddedBridge({
  appSessionCookieName: "mk_app_session",
  socialCallbackPath: "/api/auth/embedded/social/callback",
});
```

Thin route wrappers should delegate directly to SDK handlers:

- `bridge.session`
- `bridge.signIn`
- `bridge.signUp`
- `bridge.forgotPassword`
- `bridge.resetPassword`
- `bridge.providers`
- `bridge.socialBegin`
- `bridge.socialCallback`
- `bridge.signOut`

## Security boundaries

- Never expose `C9D_SESSION_SECRET` or `C9D_OAUTH_CLIENT_SECRET` in client code.
- Do not create `NEXT_PUBLIC_C9D_OAUTH_CLIENT_SECRET`.
- Keep token exchange server-side only.

## Product usage tracking

Product usage is tracked automatically by the identity host. When a product app exchanges an authorization code at the token endpoint, the identity host records the user-product association via a Prisma query extension on `OAuthAccessToken.create`. No manual `POST /api/auth/track-product` call is needed from `@c9d/auth-next` apps.

## Hosted documentation

- SDK docs hub: `https://identity.c9d.engineering/docs/sdk`
- Next.js server guide: `https://identity.c9d.engineering/docs/sdk/nextjs-server`
- Product bridge quickstart: `https://identity.c9d.engineering/docs/sdk/product-bridge-quickstart` (or repo doc `packages/auth-next/docs/PRODUCT_BRIDGE_QUICKSTART.md`)
- Downloadable copy of this skill: `https://identity.c9d.engineering/resources/cursor/c9d-auth-next/SKILL.md`
