# [Monorepo Name]
[One-sentence: what does this monorepo contain?]

Monorepo with [package manager: pnpm/yarn/npm workspaces] + [Turborepo/Nx/Lerna].

## Packages
- `[packages/app1]` — [purpose]
- `[packages/app2]` — [purpose]
- `[packages/shared]` — [purpose; depended on by app1 and app2]

## Commands (run from root)
- `pnpm dev` — start all apps
- `pnpm dev --filter=@org/app1` — start one app
- `pnpm test` — test all packages
- `pnpm test --filter=@org/app1` — test one package
- `pnpm build --filter=@org/app1` — build one package
- `pnpm lint && pnpm typecheck && pnpm test` — required before commit

## Universal Conventions
- TypeScript strict mode in all packages
- Conventional commits (`feat:`, `fix:`, `chore:`, etc.)
- Each package has its own `README.md` and `CLAUDE.md`
- Shared types live in `packages/shared/src/types/`
- Inter-package imports use workspace protocol: `"@org/shared": "workspace:*"`

## Rules
- Never duplicate code across packages; promote to `packages/shared`
- Never import private internals from another package (only from its public exports)
- All packages export through a single `index.ts`
- **IMPORTANT:** Run `pnpm install` after any package.json change

## Gotchas
- Package versions managed by [changesets/release-please]; do not edit package.json versions directly
- [Build order matters for X reason]
- [Specific package has Y quirk]

## See also
- Each package has its own `CLAUDE.md` with package-specific rules
- Architecture: `docs/monorepo-architecture.md`
- Package boundary rules: `docs/package-boundaries.md`

---

## Sub-Package Template

For each `packages/<name>/CLAUDE.md`, use this minimal structure:

```markdown
# @org/<package-name>
[One-sentence purpose. Supplements root CLAUDE.md.]

## Package-specific
- [Rule that applies only to this package]
- [Convention specific to this package's domain]

## Internal Structure
- `src/[subfolder]/` — [purpose]

## Gotchas
- [Package-specific gotcha]

## See also
- Root `CLAUDE.md` for monorepo-wide conventions
```
