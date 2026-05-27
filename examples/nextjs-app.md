# Example: Next.js App (Standard Pattern)

A typical CLAUDE.md for a Next.js e-commerce app. Shows balanced use of all sections.

## The File

```markdown
# Acme Shop
Next.js 15 e-commerce app with Stripe checkout, Prisma 6, PostgreSQL 16.
Deployed to Vercel.

## Commands
- `pnpm dev` — start dev server (port 3000)
- `pnpm test` — run all tests (Vitest)
- `pnpm test -- src/api/checkout.test.ts` — run a single test file
- `pnpm db:migrate` — apply Prisma migrations
- `pnpm typecheck && pnpm lint` — required before commit

## Structure
- `app/` — Next.js App Router pages and layouts
- `lib/` — shared utilities; put new ones here, not in components
- `lib/stripe/` — Stripe SDK wrappers (use these, not stripe directly)
- `prisma/` — schema and migrations (run `pnpm db:migrate` after edits)

## Rules
- TypeScript strict mode; no `any` (use `unknown` with narrowing)
- Use named exports, not default exports
- Server components by default; mark client with `"use client"` only when needed
- All API responses follow shape: `{ data, error, status }`
- Use `lib/logger.ts`, not console.log
- Never use `--legacy-peer-deps`; resolve conflicts by upgrading

## Gotchas
- Stripe webhook handler in `app/api/webhooks/stripe/route.ts` must validate
  signature using `STRIPE_WEBHOOK_SECRET`
- Product images live in Cloudinary, not `/public`; use `<CldImage>` component
- Prisma's `$transaction` does not work with edge runtime; check `runtime` config
- Server actions automatically revalidate; explicit `revalidatePath` only for client-triggered updates

## See also
- Architecture: read `docs/architecture.md` when modifying checkout flow
- Stripe integration: read `docs/stripe-guide.md` for webhook/refund/dispute patterns
- Database conventions: `docs/db-conventions.md`
```

## Annotations

**Lines 1-3 (identity)**: Tech stack with versions is critical. "Next.js 15" vs "Next.js 14" changes which APIs Claude suggests. Always include versions.

**Lines 5-10 (commands)**: Note `pnpm test -- src/api/checkout.test.ts` — single-test syntax is non-obvious and easy to mistype. Worth a line. Generic `pnpm install` is NOT included because Claude knows that.

**Lines 12-17 (structure)**: Only non-obvious entries. `app/` is mentioned because the rule "App Router, not Pages Router" matters. `lib/` has a usage rule attached ("put new utilities here, not in components"). `lib/stripe/` redirects from raw SDK usage. `prisma/` reminds about migration step.

**Lines 19-25 (rules)**: 6 rules total, all project-specific. Paired prohibition + direction throughout ("Never X; use Y instead"). No formatting rules (linter handles those). No personality.

**Lines 27-32 (gotchas)**: 4 gotchas, each tracing to a real issue. The Stripe webhook one likely saved someone an hour of debugging. The edge runtime + Prisma one is a specific incompatibility. Each one passes the pruning test.

**Lines 34-37 (pointers)**: Three pointers using prose ("read X when Y"), not `@imports`. This means the docs only load when relevant — saves context budget. Note the specificity: "when modifying checkout flow", not just "for architecture stuff".

## What's notably absent

- ❌ "You are a senior engineer" — personality bloat
- ❌ "Use 2-space indentation" — Prettier handles it
- ❌ "Write meaningful variable names" — generic advice
- ❌ Long code samples — Claude already knows syntax
- ❌ Marketing description ("Acme Shop is a comprehensive solution...") — README's job

## Size analysis

| Section | Lines |
|---|---|
| Title + identity | 3 |
| Commands | 6 |
| Structure | 5 |
| Rules | 7 |
| Gotchas | 5 |
| Pointers | 4 |
| Section spacing | 11 |
| **Total** | **~55 lines** |

Well under 80-line target. Every line is project-specific.

## When to evolve

After 3 months of use, this file likely needs:
- Maybe 1-2 new gotchas added (real incidents)
- Maybe 1-2 stale rules removed (refactored away)
- Architecture section may have grown, justifying split to `docs/architecture.md`

A healthy CLAUDE.md is a living document, not a frozen artifact.
