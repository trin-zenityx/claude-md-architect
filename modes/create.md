# Mode: Create

Create a new CLAUDE.md (and optional supporting docs/ structure) for a project from scratch.

Use this mode when the user wants to set up Claude Code context for a new or existing project that doesn't yet have a CLAUDE.md, and is comfortable starting fresh rather than extracting from a specific past session.

## Workflow

### Phase 1: Pre-flight Discovery (5–10 min)

Before asking the user anything, gather what you can from the codebase:

1. **List the project root** with `Bash ls` (or the Glob/Read tools) to see structure
2. **Read README.md** if it exists
3. **Identify the stack** by checking config files:
   - `package.json` (Node/JS — note `dependencies`, `scripts`, version)
   - `pyproject.toml` / `requirements.txt` (Python)
   - `Cargo.toml` (Rust)
   - `go.mod` (Go)
   - `composer.json` (PHP)
   - `Gemfile` (Ruby)
   - Configuration for frameworks (`next.config.js`, `vite.config.ts`, etc.)
4. **Check for existing context files**: `.cursor/rules`, `.cursorrules`, `.github/copilot-instructions.md`, `AGENTS.md` — these may contain knowledge to migrate
5. **Quick git log** with `git log --oneline -20` to understand recent activity (skip if non-git project)
6. **Look for non-obvious patterns**: monorepo? unusual folder layout? custom build pipeline?

This gives you 70% of the information needed. Do NOT ask the user things you can answer from files.

### Phase 2: Targeted Interview (5–15 min)

Now ask the user only what you can't infer. Use `AskUserQuestion` tool if available. Ask in batches of 2–3, not one at a time.

**Always ask:**
1. "What does this project do, in 1–2 sentences? (the 'why' — what problem it solves)"
2. "Are there gotchas or non-obvious patterns I should know? (Things that wasted time before)"
3. "Any conventions you enforce that aren't in a linter? (Naming, error handling shape, etc.)"

**Ask if relevant:**
4. (If commands look unusual): "Walk me through your typical command sequence — what do you run before commit?"
5. (If monorepo): "Should I create per-package CLAUDE.md files too?"
6. (If non-code): "Who's the audience for the output? Voice/tone constraints?"
7. (If team project): "Are there shared conventions vs your personal preferences?"

**Never ask:**
- Things visible in `package.json` (stack, dependencies, scripts)
- Things visible in folder structure
- Generic questions like "what's important to you"

### Phase 3: Decide Scope

Based on Phase 1+2 findings, decide:

**Single CLAUDE.md only** when:
- Small project (<50 source files)
- Single concern (one app, one library)
- User is beginner (less is more)
- Total content fits comfortably in 60 lines

**CLAUDE.md + docs/** when:
- Content naturally exceeds 80 lines
- Has gotchas in a specific domain (e.g., LINE Bot API, image generation prompts)
- Has architectural decisions worth recording

**CLAUDE.md + per-directory CLAUDE.md** when:
- Monorepo with packages that have distinct concerns
- Different team owns different subdirectories
- src/api/ and src/frontend/ have very different rules

**CLAUDE.md + CLAUDE.local.md** when:
- User mentions personal preferences they don't want to commit
- User has API keys, local DB paths, machine-specific config

### Phase 4: Draft Using a Template

Pick the closest template from `templates/`:

| Project type | Template |
|---|---|
| Simple web/CLI app | `standard.md` |
| Just a single script or learning project | `minimal.md` |
| Monorepo | `monorepo.md` |
| Blog/content/writing project | `non-code-content.md` |
| Marketing/PM workspace (non-coding) | `non-code-content.md` (adapt) |
| Data science / research | `non-code-research.md` |
| User explicitly wants self-maintenance | `self-improving.md` |

**Read the template, then adapt — do not copy verbatim.**

Adaptation rules:
1. Replace placeholders with actual values from Phase 1+2
2. Delete sections that aren't applicable
3. Add gotchas from Phase 2 (these are the highest-value content)
4. Ensure final file is ≤80 lines

### Phase 5: Write the Files

Create the files. Use:
- `Write` for new files (or `Edit` if editing an existing CLAUDE.md)
- Default path: project root for `CLAUDE.md`, `project-root/docs/` for supporting

**Order:**
1. `CLAUDE.md` first
2. Any `docs/` files referenced from CLAUDE.md
3. `.gitignore` entry if CLAUDE.local.md is created
4. (Optional) `CLAUDE.local.md` template if user has personal preferences

### Phase 6: Self-Test (CRITICAL — DO NOT SKIP)

After writing, simulate a new session:

> "If I were a Claude session opening this project for the first time, having read only the files just created, I would understand:
> 1. The project does X
> 2. The stack is Y
> 3. Key gotchas are Z
> 4. I should ALWAYS do A and NEVER do B"

State this back to the user. Ask: "Does this match what you'd want a new session to know? Anything missing or wrong?"

This catches gaps before the user discovers them in real use.

### Phase 7: Commit Suggestion

Suggest the user commit:

```bash
git add CLAUDE.md docs/ .gitignore
git commit -m "Add Claude Code context system"
```

If CLAUDE.local.md exists, remind: "Don't commit CLAUDE.local.md — confirm it's in .gitignore."

## Variations

### Variation: Migrating from .cursorrules / Copilot Instructions

If Phase 1 found existing AI tool config:

1. Read those files first
2. Extract substantive rules (skip personality/role priming)
3. Translate to CLAUDE.md format (paired prohibition+direction, bullets, etc.)
4. Apply size limits — old tool configs often violate them

### Variation: New Project (No Code Yet)

If the project is brand new with mostly empty repo:

1. Skip Phase 1 codebase inspection (nothing to read)
2. Phase 2 becomes longer — ask about intended stack, architecture
3. Mark CLAUDE.md as "scaffolding" — note it will evolve
4. Suggest re-running this mode after Week 1 to refine based on actual decisions

### Variation: Brownfield Project (Old, Many Conventions)

If the project is years old with many implicit conventions:

1. Phase 1 will yield more than usual — read deeply
2. Phase 2: ask "what mistakes have new contributors made?" — these reveal hidden rules
3. Start with a minimal CLAUDE.md, then offer to do a follow-up audit mode after the user uses Claude on the project for 1–2 weeks
4. Don't try to capture everything in v1

## Anti-Patterns to Avoid in Create Mode

❌ **Generating from `/init` and committing unchanged** — the output is a draft, not a finished file. The ETH Zurich AGENTS.md study found low-quality auto-generated context files actively *reduce* task success rates and increase cost by 20%+.

❌ **Asking the user to fill out a long questionnaire** — they don't know what they don't know. Read first, ask sparingly.

❌ **Including everything "just in case"** — every line dilutes attention. Less is more.

❌ **Skipping Phase 6 self-test** — this is the single best quality check. Always do it.

❌ **Creating empty docs/ files pre-emptively** — only create `docs/decisions.md`, `docs/gotchas.md`, etc. if there's actual content. Empty files are noise.

## Quality Checklist Before Finishing

Before declaring done, confirm:

- [ ] CLAUDE.md is ≤80 lines (or ≤150 with strong justification)
- [ ] Every line passes the pruning test
- [ ] No code-style rules a linter would enforce
- [ ] No personality/role priming
- [ ] Pairs prohibitions with directions
- [ ] Phase 6 self-test passed
- [ ] User has been told to commit
- [ ] CLAUDE.local.md is in .gitignore if it exists
