# Anti-Patterns Reference

The 10 documented anti-patterns in CLAUDE.md design, with examples and fixes.

Use this reference when:
- Auditing an existing CLAUDE.md
- Explaining to a user why something they want to add shouldn't go in

## 1. The Bloated Catch-All

**Symptom**: 500+ line CLAUDE.md with formatting rules, war stories, every command, every preference.

**Why it fails**: HumanLayer's analysis (Mistele, [humanlayer.dev/blog/writing-a-good-claude-md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)) finds that as instructions accumulate, the LLM doesn't gracefully ignore the *newer* ones — it begins ignoring *all of them uniformly*: *"instruction-following quality decreases uniformly … it begins to ignore all of them uniformly."* Even frontier models max out around 68% accuracy at 500 instructions (Jaroslawicz et al., IFScale, arXiv:2507.11538), and degradation starts long before that. Claude Code's system prompt already uses tokens before you add anything.

**Bad**:
```markdown
# My App

## Code Style
- 2-space indentation
- Single quotes
- Trailing commas in multiline
- Semicolons required
- camelCase for variables
- PascalCase for components
- ... [40 more lines of style rules]

## Architecture
[100 lines explaining the system]

## Database
[80 lines explaining the schema]

## Testing
[60 lines on testing philosophy]

## Personal Preferences
[40 lines of "I like to..."]
```

**Fix**:
- Move code style → `.editorconfig` + ESLint + Prettier
- Move architecture → `docs/architecture.md`, pointer from CLAUDE.md
- Move database → `docs/db-schema.md` or just point to schema file
- Move personal preferences → `CLAUDE.local.md`
- Result: CLAUDE.md becomes 50 lines of project-specific essentials

## 2. Claude As Linter

**Symptom**: Formatting rules in CLAUDE.md.

```markdown
- Use 2-space indentation
- Always use single quotes for strings
- Add trailing commas in multiline arrays
- No semicolons at end of statements
- Spaces around operators
```

**Why it fails**: HumanLayer's blunt take (section heading: *"Claude is (not) an expensive linter"*): *"Never send an LLM to do a linter's job."* LLMs are slow and expensive compared to traditional linters. They also miss formatting violations sometimes.

**Fix**: Configure `.editorconfig`, `.eslintrc`, `.prettierrc`, `pyproject.toml [tool.black]`, etc. Add a Stop hook or pre-commit hook to run them automatically. Delete the rules from CLAUDE.md entirely.

## 3. Auto-Generated and Untouched

**Symptom**: 200-line output from `claude /init` committed without edits. Contains generic advice Claude already knows ("write clear code", "use meaningful names", "follow conventions").

**Why it fails**: HumanLayer: *"CLAUDE.md is the highest leverage point of the harness, so avoid auto-generating it."* The ETH Zurich / LogicStar evaluation (Gloaguen et al., "Evaluating AGENTS.md", arXiv:2602.11988, ICLR 2026 workshop) tested 4 frontier models against 138 real Python tasks across 3 settings (no context file, LLM-generated, human-written). Finding: **context files tend to REDUCE task success rates compared to no repository context, while increasing inference cost by over 20%**. The mechanism: generic boilerplate dilutes the model's attention away from the specific signals it would otherwise pick up from code.

**Fix**: Treat `/init` output as a *draft*, not a finished file. Read every line. Delete anything Claude already knows. Keep only project-specific content. Typical result: cut 60–80% of the auto-generated file.

## 4. Negation-Only Constraints

**Symptom**: Rules say what NOT to do, without saying what to do instead.

**Bad**:
```markdown
- Never use class components.
- Never use --legacy-peer-deps.
- Don't use console.log.
- Avoid any.
```

**Why it fails**: Claude knows what's prohibited but defaults to its most common training pattern (which may be exactly what you're trying to avoid). Or it gets stuck deciding.

**Fix**: Pair every prohibition with a direction.

**Good**:
```markdown
- Never use class components; use function components with hooks.
- Never use --legacy-peer-deps; resolve conflicts by upgrading the conflicting package.
- Don't use console.log; use the logger at `src/utils/logger.ts`.
- Avoid `any`; if a type is truly unknown, use `unknown` with explicit narrowing.
```

## 5. Personality Bloat

**Symptom**: Role priming, persona, "you are a..."

```markdown
You are a senior 10x engineer who thinks step by step.
You write clean, maintainable code.
You always consider edge cases.
You are an expert in TypeScript, React, and Node.js.
```

**Why it fails**: Claude Code's own system prompt already includes strong role priming. Adding more doesn't help — it just consumes tokens and dilutes attention.

**Fix**: Delete. All of it. Replace with concrete project-specific rules only.

## 6. Stale and Contradictory Rules

**Symptom**: Rules conflict with each other, reference deleted modules, or describe outdated architecture.

**Bad** (excerpt from real-world examples):
```markdown
- Use Redux for state management.
- ...
- For all new components, use Zustand stores.
- ...
- The legacy `useAuthHook` should be replaced with `useAuthContext`.
```

(All three coexist in the file — Redux for some, Zustand for others, and `useAuthHook` was deleted 6 months ago.)

**Why it fails**: Anthropic memory docs: *"If two files give different guidance for the same behavior, Claude may pick one arbitrarily."* Claude can't tell which rule is current, so it picks unpredictably.

**Fix**:
- Quarterly review: read CLAUDE.md against current code, delete stale rules
- When you change direction, immediately update CLAUDE.md (not "I'll do it later")
- Add a "Last reviewed: YYYY-MM-DD" line at the end as a reminder

## 7. Conflicting Tool Instructions

**Symptom**: User-level CLAUDE.md and project-level CLAUDE.md disagree.

`~/.claude/CLAUDE.md`:
```markdown
- Use tabs for indentation
- Prefer Vue
```

`./CLAUDE.md`:
```markdown
- Use 2-space indentation
- This is a React project
```

**Why it fails**: Both load into context. Claude has to resolve the conflict and often resolves wrong, or alternates inconsistently.

**Fix**:
- Keep `~/.claude/CLAUDE.md` minimal: only truly universal preferences (5–15 lines)
- Make project CLAUDE.md the source of truth for project-specific decisions
- Don't put "preferences" in user CLAUDE.md — only personal *style* that applies everywhere (e.g., "respond in concise prose, not bullet lists, when explaining" — this can apply across all projects)

## 8. Adding a Rule for Every One-Off Mistake

**Symptom**: 300-line "Gotchas" section that grows forever. Most entries reference incidents that happened once.

**Why it fails**: DataCamp: *"A new line belongs in the file only when Claude makes an actual mistake that line would have prevented. Every rule should trace back to a real incident, not a hypothetical one."* But: even one real incident isn't enough. One-offs add noise.

**Fix**: The promotion rule:
- One-off mistake → leave in auto-memory (MEMORY.md), don't add to CLAUDE.md
- Same mistake 2+ times → promote to CLAUDE.md gotchas section
- Trivial mistakes → just correct Claude inline, never add to file

Periodically (quarterly) prune the gotchas section. Many entries become obsolete as code evolves.

## 9. Importing Everything via @

**Symptom**: Top of CLAUDE.md is a wall of imports.

```markdown
@docs/architecture.md
@docs/api-conventions.md
@docs/database.md
@docs/auth.md
@docs/payments.md
@docs/style-guide.md
@docs/testing.md
@docs/deployment.md
```

**Why it fails**: Anthropic memory docs: *"Splitting into `@path` imports helps organization but does not reduce context, since imported files load at launch."* This pattern is worse than monolithic — it gives the illusion of organization while loading just as much content into every session.

**Fix**: Distinguish two cases:

- **Content needed every session** → keep in CLAUDE.md directly, OR `@import`
- **Content needed only sometimes** → prose pointer: "For payment issues, read `docs/payments.md`"

For most cross-cutting concerns, prose pointer is correct. Claude reads the doc only when relevant. This is true progressive disclosure.

## 10. CLAUDE.md as Documentation

**Symptom**: CLAUDE.md reads like a README.

```markdown
# My App

My App is a comprehensive solution for managing customer relationships in the
e-commerce space. Built with modern technologies, it provides a seamless
experience for users while maintaining the highest standards of security and
performance.

## Features
- Customer management
- Order tracking
- ...

## Installation
First, clone the repository:
```bash
git clone ...
```

Then install dependencies:
```

**Why it fails**: TurboDocx: *"It's not documentation — it's a context injection file."* README is for humans reading on GitHub. CLAUDE.md is for Claude's working memory. Different audiences, different content.

**Fix**:
- README.md (for humans): marketing description, installation, contribution guide, license
- CLAUDE.md (for Claude): commands, conventions, gotchas, pointers to deeper docs

Strip everything human-marketing-flavored from CLAUDE.md. Replace with direct, terse, actionable content.

## Detection Checklist

When auditing, look for these specific signs:

| Sign | Likely Anti-Pattern |
|---|---|
| File over 200 lines | #1 Bloat |
| Many lines about formatting/style | #2 Linter rules |
| Looks like `/init` output | #3 Auto-generated |
| Multiple "Never" without "Instead" | #4 Negation-only |
| Lines start "You are..." | #5 Personality |
| Conflicting guidance for same thing | #6 Stale |
| User CLAUDE.md disagrees with project | #7 Tool conflict |
| Gotchas section over 50 lines | #8 One-off accumulation |
| Many @imports at top | #9 Import abuse |
| Reads like marketing copy | #10 README-style |

If a CLAUDE.md has 3+ signs, full audit + refactor is needed. 1–2 signs, targeted fix.
