# Decision Framework: Where Does Content Go?

When the user asks "should this go in CLAUDE.md?", use this framework to decide. Show the user the reasoning, not just the answer — they'll learn to make these decisions themselves.

## Main Decision Tree

```
[Piece of content to file]
        │
        ▼
1. Can Claude infer this from reading the code?
        │
        ├─ YES → Don't file it anywhere. It's noise.
        │
        ▼ NO
2. Will a linter/formatter/CI catch violations?
        │
        ├─ YES → Configure that tool. Don't put in CLAUDE.md.
        │
        ▼ NO
3. Does it apply to EVERY session in this project?
        │
        ├─ YES → CLAUDE.md (root)
        │
        ▼ NO
4. Does it apply to SOME specific subdirectory?
        │
        ├─ YES → subdirectory CLAUDE.md or .claude/rules/ with paths frontmatter
        │
        ▼ NO
5. Is it a multi-step workflow with a clear name?
        │
        ├─ YES → .claude/commands/<name>.md (slash command)
        │
        ▼ NO
6. Is it deep knowledge that's sometimes needed?
        │
        ├─ YES → docs/<topic>.md (referenced by prose pointer)
        │
        ▼
7. Is it personal preference / machine-specific?
        │
        ├─ YES → CLAUDE.local.md (.gitignored)
        │
        ▼ NO
8. Is it a reusable capability across projects?
        │
        ├─ YES → ~/.claude/skills/<name>/SKILL.md
        │
        ▼
   Unclear — ask the user
```

## Detailed Decision Table

| Content type | Goes in | Why |
|---|---|---|
| Project's one-line purpose | `CLAUDE.md` | Always needed |
| Tech stack with versions | `CLAUDE.md` | Always needed; versions matter |
| Bash commands (custom) | `CLAUDE.md` | Claude can't guess your scripts |
| Bash commands (standard) | Nowhere | `npm install` is universal knowledge |
| Folder structure (non-obvious) | `CLAUDE.md` | Prevents misplaced files |
| Folder structure (standard) | Nowhere | `src/`, `tests/` is obvious |
| Top 5 conventions | `CLAUDE.md` | Always needed |
| Code style (indent, quotes, etc.) | `.editorconfig` / ESLint | Linter's job |
| Top 5 gotchas | `CLAUDE.md` | Always needed |
| All gotchas (50+) | `docs/gotchas.md` | Too much for CLAUDE.md |
| Architectural rationale | `docs/architecture.md` | Read on-demand |
| Architectural decisions log | `docs/decisions.md` (or ADR) | Read on-demand |
| Domain-specific patterns | `docs/<domain>.md` or skill | On-demand |
| API conventions | `docs/api.md` referenced by prose | Sometimes needed |
| Workflow ("how to release") | `.claude/commands/release.md` | Invoked explicitly |
| Personal style preference | `~/.claude/CLAUDE.md` | Cross-project |
| Personal project shortcut | `CLAUDE.local.md` | Per-project, personal |
| Stripe API workflow | `.claude/skills/stripe/` | Reusable, on-demand |
| Production URL | `docs/environments.md` | Reference |
| Stack trace from one bug | Nowhere | Trivia, not a rule |
| Bug fix from same recurring issue | `docs/gotchas.md` | Pattern, file it |
| Secret / API key | **NEVER ANYWHERE** | Use env vars |

## Decision Heuristics

### Heuristic 1: The Pruning Test

For any candidate line in CLAUDE.md, ask:

> "If I removed this line, would Claude make a mistake?"

- YES → it belongs
- MAYBE → cut it; if Claude makes the mistake later, add it back
- NO → definitely cut

### Heuristic 2: The Linter Test

For any candidate rule, ask:

> "Could a linter, formatter, type checker, or CI catch a violation?"

- YES → configure that tool. Delete the rule from CLAUDE.md.
- NO → CLAUDE.md is a valid place

A useful framing from the community: *"If a violation would block a merge in CI, the rule belongs in CI. If a violation would make a code reviewer raise an eyebrow, the rule belongs in CLAUDE.md."*

### Heuristic 3: The Frequency Test

For any candidate content:

> "How often will this matter in a typical session?"

- Every session → CLAUDE.md
- Some sessions → docs/ with prose pointer
- Rare sessions → docs/ or skill, definitely not always-loaded

### Heuristic 4: The Audience Test

> "Who is the audience for this content?"

- Humans (contributors, users) → README.md
- Claude → CLAUDE.md or supporting docs
- Both → write twice with different framing; don't share

### Heuristic 5: The Inference Test

> "Could Claude figure this out by reading 2 files of code?"

- YES → don't write it. Trust the code.
- NO → it's a candidate for CLAUDE.md

This catches generic best practices Claude already knows.

## Common Borderline Cases

### "Use TypeScript" — does it belong?

Depends on visibility:
- If `tsconfig.json` exists and `.ts` files dominate → Claude infers. Don't say it.
- If project is mixed JS/TS and the rule is "new code = TS" → say it; not inferable

### "Use Prettier" — does it belong?

No. Either Prettier is in dependencies (Claude can see), or it's not. Either way, the formatter does the work.

### "Don't break existing tests" — does it belong?

No. Generic principle Claude knows. Doesn't pass the inference test.

### "Run pnpm typecheck before committing" — does it belong?

Yes. Custom command, specific workflow, not obvious from project structure.

### "We deploy on Vercel" — does it belong?

Borderline. If `vercel.json` exists, Claude infers. If deployment behavior matters (e.g., "edge runtime only" or "specific env vars required"), make that explicit. Don't say "we deploy on Vercel" by itself.

### "Customer IDs are strings, not integers" — does it belong?

Yes. Non-obvious data quirk. Exactly the kind of gotcha CLAUDE.md is for.

### "Be careful with race conditions" — does it belong?

No. Generic advice. Replace with: "X module has known race condition risks; use the locking helper in `src/utils/lock.ts`." Now it's specific.

### "API responses should be in JSON" — does it belong?

No. Universal default. Claude knows.

### "API responses use the shape `{ data, error, status }`" — does it belong?

Yes. Project-specific convention not visible from a single endpoint's code.

## When User Disagrees

Some users want to "be thorough" and resist cutting. Respond:

1. **Validate**: "I get that you want safety. Let me explain the failure mode."
2. **Explain**: "As instruction count grows, Claude doesn't ignore *new* ones — it ignores *all* of them uniformly. The IFScale benchmark (arXiv:2507.11538) shows frontier models hit just 68% accuracy at 500 instructions, and degradation starts well before. HumanLayer documented this in their analysis of real production sessions. More rules = fewer rules followed."
3. **Offer compromise**: "Keep the lean CLAUDE.md. Move the rest to docs/ with prose pointers. You get thoroughness AND adherence."

If they still want to overload CLAUDE.md, write it — but include a brief note that this is against best practice. Then respect their choice.

## Special Cases

### Monorepo

| Content | Location |
|---|---|
| Universal rules (TypeScript, conventions) | Root CLAUDE.md |
| Package-specific rules | `packages/<name>/CLAUDE.md` |
| Shared docs | `docs/` at root |
| Per-package docs | `packages/<name>/docs/` |

### Multi-Language Project

Either:
- One CLAUDE.md per language directory, OR
- Single root with language-specific subsections

Choose based on whether the languages are coupled (frontend+backend talking to each other) or independent.

### Open Source Library

| Content | Location |
|---|---|
| Public API | README.md |
| Contribution guide | CONTRIBUTING.md |
| Claude-specific dev context | CLAUDE.md |
| Architecture for maintainers | docs/architecture.md |

CLAUDE.md should NOT duplicate README; it's the "what maintainers know but isn't written down" doc.

### Internal Company Project

Add `CLAUDE.local.md` for things only your team should know but not commit (internal URLs, person names, internal jargon explanations). Make sure it's .gitignored.

## When to Promote / Demote

Content can move between files over time. Triggers:

**Promote to CLAUDE.md (from docs/ or MEMORY.md)**:
- Same mistake recurs 2+ times
- Rule is being constantly forgotten — needs always-loaded status

**Demote from CLAUDE.md**:
- Rule hasn't been violated in 3+ months → consider removing
- Detail has grown too large → move to docs/ with prose pointer
- Becomes irrelevant due to code changes → delete

**Move to skill**:
- Pattern is being reused across multiple projects
- Becomes complex enough to need its own structured guidance

The lifecycle is healthy. CLAUDE.md is not a write-once file.
