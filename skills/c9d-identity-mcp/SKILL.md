---
name: c9d-identity-mcp
description: Use when the user asks to query or change C9D Identity users, sessions, OAuth clients, products, environments, organizations, or audit logs via MCP or a Personal Access Token against the identity platform.
---

# C9D Identity MCP workflows

## Before you start

1. Confirm target host (production vs preview) and that `MCP_ADMIN_ENABLED=true`.
2. Prefer read tools (`list_*`, `get_*`) before writes.
3. Fine-grain mode: ensure the PAT includes the scopes for each tool (`lib/mcp-scope-policy.ts`).

## Workflow: Product + OAuth

1. `list_products` or create hierarchy: `create_product` → `create_application` → `create_environment`.
2. `create_oauth_client` — capture `clientSecret` once securely.
3. `update_environment` with `oauthClientId` to link; set `trustedOrigins` per environment.
4. Verify with `list_oauth_clients` / `list_products`.

## Workflow: Users

1. `list_users` or `get_user` to resolve identity.
2. Apply writes: `create_user`, `update_user`, `ban_user`, `delete_user`, etc. (`delete_user` cannot target the PAT holder).
3. Expect audit entries with `channel=mcp`.

## Workflow: OAuth clients

1. `list_oauth_clients` / `get_oauth_client` for current state.
2. `create_oauth_client` or `update_oauth_client`; use `rotate_oauth_client_secret` when rotating.
3. Optionally link to an environment with `update_environment`.
4. Never log or paste `clientSecret` from tool results.

## Workflow: Organizations

1. `list_organizations` / `get_organization` / `list_organization_members`.
2. `create_organization` or member changes with `add_organization_member`, `update_organization_member_role`, `remove_organization_member`.

## When MCP is disabled

Operators see `MCP_ROUTE_DISABLED`. Use Admin UI or REST admin APIs instead.

## Workflow: Product analytics

1. `get_oauth_client` returns `userCount` and `activeSessionCount` for any product's OAuth client.
2. The admin dashboard (`/admin`) supports a product selector dropdown to filter all analytics by product.
3. `GET /api/admin/analytics?clientId=<public-clientId>` returns product-scoped KPIs, time-series, retention, and recent users.
4. The analytics response always includes a `products` array with per-client summary (userCount, activeSessionCount, newUsersWeek).

## Reference

- Policy and tables: `docs/admin-guide/MCP_SERVER.md`
- In-app summary (login required): `/admin/ai-resources`
