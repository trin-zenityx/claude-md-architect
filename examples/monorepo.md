# Example: Monorepo (Root + Sub-Package)

Demonstrates the root + per-package overlay pattern. Each package CLAUDE.md is loaded only when Claude works in that subdirectory.

## Root CLAUDE.md

```markdown
# Acme Platform
TypeScript monorepo with web app, mobile app, and shared packages.

Stack: pnpm workspaces + Turborepo, TypeScript strict everywhere.

## Packages
- `packages/web` — Next.js customer-facing site
- `packages/mobile` — React Native app  
- `packages/api` — Express API server
- `packages/shared` — types and utilities (depended on by all)

## Commands (from root)
- `pnpm dev` — start everything
- `pnpm dev --filter=@acme/web` — start one app
- `pnpm test --filter=@acme/api` — test one package
- `pnpm lint && pnpm typecheck && pnpm test` — required before commit

## Universal Conventions
- TypeScript strict, no `any`
- Conventional commits (feat:, fix:, chore:, ...)
- Each package has its own README and CLAUDE.md
- Workspace protocol for internal deps: `"@acme/shared": "workspace:*"`
- Shared types: `packages/shared/src/types/`

## Rules
- Never duplicate code; promote to `packages/shared`
- Never import from another package's internals; use public exports only
- Package versions managed by changesets; don't hand-edit package.json
- **IMPORTANT:** Run `pnpm install` after any package.json change

## Gotchas
- Build order matters: shared must build before web/mobile/api
- `packages/mobile` requires native build setup; see docs/mobile-setup.md

## See also
- Each `packages/<name>/CLAUDE.md` for package-specific rules
- Architecture: `docs/monorepo-architecture.md`
```

**Root: ~35 lines.** Covers cross-package concerns only.

## packages/api/CLAUDE.md

```markdown
# @acme/api
Express REST API server. Supplements root CLAUDE.md.

## Commands (run with --filter=@acme/api from root)
- `pnpm dev` — start API on port 4000
- `pnpm test:integration` — integration tests with test DB

## Internal Structure
- `src/routes/` — route handlers (one file per resource)
- `src/middleware/` — Express middleware
- `src/services/` — business logic (testable, no Express deps)
- `src/db/` — Prisma client and queries

## API-specific Rules
- Validate request bodies with Zod schemas in `src/schemas/`
- Wrap async handlers with `asyncHandler()` for error capture
- All responses use `{ data, error, status }` shape
- Authentication via JWT in `Authorization: Bearer` header

## Gotchas
- Prisma client is a singleton; don't instantiate in route handlers
- Rate limiter applies after auth middleware; order matters in app.ts

## See also
- Root CLAUDE.md for monorepo-wide rules
- API documentation generated to `docs/api-spec.md`
```

**packages/api/: ~25 lines.** Only API-specific content.

## packages/mobile/CLAUDE.md

```markdown
# @acme/mobile
React Native app for iOS and Android. Supplements root CLAUDE.md.

## Mobile-specific Setup
- Requires Xcode 16+ (iOS) and Android Studio Hedgehog+ (Android)
- Run `pnpm pod-install` after adding native dependencies

## Commands
- `pnpm ios` — run iOS simulator
- `pnpm android` — run Android emulator
- `pnpm e2e` — Detox end-to-end tests

## Internal Structure
- `src/screens/` — screen components
- `src/navigation/` — React Navigation config
- `src/services/api.ts` — wraps @acme/api client

## Mobile-specific Rules
- Use React Native primitives (View, Text), not HTML elements
- All async operations show loading state via the shared `<Loading/>` component
- Test on both platforms before merging mobile-touching PRs

## Gotchas
- iOS Simulator does not have keychain by default; mock in test setup
- Android requires SDK 34+; older devices not supported

## See also
- Root CLAUDE.md for monorepo-wide rules
- Native setup details: `docs/mobile-setup.md`
```

**packages/mobile/: ~28 lines.** Mobile-only content.

## How loading works

When Claude works on `packages/api/src/routes/users.ts`:
- Root `CLAUDE.md` loads (always)
- `packages/api/CLAUDE.md` loads (Claude is in this subdirectory)
- `packages/mobile/CLAUDE.md` does NOT load (not relevant)

Total context: ~60 lines (35 + 25). Each section addresses what's needed.

When Claude works on `packages/web/src/components/Cart.tsx`:
- Root loads
- `packages/web/CLAUDE.md` loads (if exists)
- API and mobile don't load

This is the power of subdirectory CLAUDE.md: scoped context, only what's needed.

## What makes this work

1. **Root has only cross-cutting content** — anything truly universal lives there
2. **Sub-package files only add, never repeat** — they assume root is loaded
3. **Each sub-package file points to root** — `## See also` reminds Claude of inheritance
4. **No package CLAUDE.md exceeds 30 lines** — staying lean at each level
5. **Total context for any one task stays under 80 lines** — attention budget preserved

## What to watch for

- **Don't repeat content** in root and sub-files
- **Don't put package-specific stuff in root** (other packages would inherit it)
- **Don't make sub-files larger than root** (defeats the purpose)
- **Do create sub-files only when needed** (small packages may not need one)
