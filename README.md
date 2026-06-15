# C9D Identity — Agent Skills

Ready-made Cursor/Claude **skills** and **rules** for building on
[C9D Identity](https://identity.c9d.engineering). Generated from the platform
repo — do not edit here; edit the source and re-run `pnpm build:agent-skills`.

## Install

```bash
# One command (any supported agent):
npx skills add C9D-Identity/agent-skills

# Or pull directly from a deployment:
curl -fsSL https://identity.c9d.engineering/api/skills/install | sh
```

## Contents

| Skill | What it covers |
|---|---|
| `c9d-identity-mcp` | Admin automation via MCP / a Personal Access Token |
| `c9d-auth-react` | `@c9d/auth-react` — sign-in UI, hooks, passkeys |
| `c9d-auth-next` | `@c9d/auth-next` — Next.js middleware, session, callbacks |
| `c9d-auth-core` | `@c9d/auth-core` — framework-agnostic server primitives |

`skills/<name>/SKILL.md` are discovered by `npx skills`. `rules/*.mdc` are
Cursor rules — copy into `.cursor/rules/`.

More: https://identity.c9d.engineering/admin/ai-resources · https://identity.c9d.engineering/llms.txt
