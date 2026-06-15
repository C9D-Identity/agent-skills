---
name: c9d-auth-react
description: Use when integrating @c9d/auth-react into a product app (C9DAuthProvider, SignIn, hooks, passkeys, theming, i18n, control components) or installing the SDK from the identity host tarball— not for MCP admin automation.
---

# C9D Identity — @c9d/auth-react (product UI)

## When to use this skill

- Building or changing **React** sign-in, sign-up, session, passkeys, or themed auth UI against **C9D Identity**.
- Installing **`@c9d/auth-react`** via HTTPS tarball from the identity deployment.

Use **`c9d-identity-mcp`** (or Admin UI) for **users, OAuth clients, products, orgs** automation.
Use **`c9d-auth-next`** for Next.js middleware/callback route implementation.
Use **`c9d-auth-core`** for framework-agnostic server auth primitives.

## Before you start

1. Resolve **identity base URL** (production, preview, or local tunnel)—no trailing slash.
2. Obtain the **public OAuth `clientId`** for your app’s environment from C9D Identity admin (product → application → environment / OAuth client).
3. Fetch **`GET https://identity.c9d.engineering/api/sdk/manifest`** for `version`, `tarball`, `install`, and `docs` URL.

## Install

```bash
pnpm add {identity_origin}{tarball_path}
```

Example after reading manifest: `pnpm add https://identity.c9d.engineering/sdk/c9d-auth-react-0.1.0.tgz`

Peer deps: `react`, `react-dom`. Optional: `@simplewebauthn/browser` for passkey flows.

## Minimal integration

1. Wrap the tree with **`C9DAuthProvider`** — required props: **`clientId`**, **`identityUrl`** (identity origin).
   - Embedded-first products should also set **`apiBaseUrl`** to product-local bridge routes (for example `/api/auth/embedded`).
   - Set **`locale`** and optional **`messages`** overrides for translated auth copy.
2. Add **`SignIn`**, **`SignUp`**, **`ForgotPassword`**, **`ResetPassword`**, or **`ProtectedRoute`** as needed.
3. Use hooks (**`useSession`**, **`useUser`**, **`useAuth`**, **`useProviders`**) and control components (**`SignedIn`**, **`SignedOut`**, **`RedirectToSignIn`**) inside the provider.

## Security

- **Never** put **PATs** or **OAuth `clientSecret`** in browser code or client bundles.
- Product apps use **`clientId` + `identityUrl`** only on the client; keep secrets on the server.

## Hosted documentation

On the identity deployment (no monorepo required):

- **SDK docs:** `https://identity.c9d.engineering/docs/sdk`
- **Components:** `https://identity.c9d.engineering/docs/sdk/components`
- **Theming + i18n:** `https://identity.c9d.engineering/docs/sdk/theming`
- **Downloadable copy of this skill:** `https://identity.c9d.engineering/resources/cursor/c9d-auth-react/SKILL.md`

## Reading order (deep dive)

1. Quick start → Components → Hooks → Theming → Auth flows (same order as `/docs/sdk` pages).
