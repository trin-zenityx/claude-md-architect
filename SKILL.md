---
name: claude-md-architect
description: Design, create, extract, audit, or teach about CLAUDE.md files and their supporting docs/ folder structure for Claude Code projects. Use this skill whenever the user asks to create a new CLAUDE.md, extract context from a long session into a CLAUDE.md system, audit/refactor an existing CLAUDE.md that has bloated, review whether their CLAUDE.md follows best practices, set up a documentation system for Claude Code, design progressive disclosure with @ imports and subdirectory CLAUDE.md files, or learn how CLAUDE.md works. Also use when user says "setup CLAUDE.md", "create context system", "fix my CLAUDE.md", "extract this conversation", "ทำ CLAUDE.md", "สร้างระบบ context", "audit ไฟล์", or mentions phrases about session continuity and project memory in Claude Code.
---

# CLAUDE.md Architect

A skill for designing CLAUDE.md files and their supporting documentation systems for Claude Code projects. Grounded in Anthropic's official memory docs (`code.claude.com/docs/en/memory`), HumanLayer's "Writing a good CLAUDE.md" guide, Vercel Engineering's January 2026 AGENTS.md eval (53% → 100% pass rate), Arize AI's prompt-learning SWE-Bench results (+10%), and the ETH Zurich AGENTS.md evaluation (Gloaguen et al., arXiv:2602.11988, ICLR 2026 workshop) — which shows that low-quality context files actively *reduce* task success and add ~20% inference cost.

## Core Mental Model

CLAUDE.md is **not** documentation — it is the highest-leverage prompt in a Claude Code project. Every line loads into every session's context and competes with the actual task for the model's attention. The single most important question for any content is: **"Would removing this cause Claude to make mistakes?"** If no, cut it.

### Three Foundational Principles

1. **Minimum Viable Context** — Anthropic targets <200 lines per file; production teams operate at 40–80. Bloat is the #1 cause of Claude ignoring rules.
2. **Progressive Disclosure** — CLAUDE.md should orchestrate, not contain. Point to deeper docs/skills/rules that load on-demand. The orchestrator pattern beats monolithic files even at identical line counts.
3. **Earn Every Line** — Each rule should trace back to a real incident, not a hypothetical one. Promote learnings from auto-memory (MEMORY.md) to CLAUDE.md only after 2+ recurrences.

## When to Use This Skill

Trigger on these user intents:

| User says... | Mode to use |
|---|---|
| "Help me create a CLAUDE.md for my new project" | **create** |
| "Set up Claude Code context for this codebase" | **create** |
| "I have a long session and want to save the context" | **extract** |
| "Before I close this chat, capture everything we discussed" | **extract** |
| "My CLAUDE.md is 500 lines — fix it" | **audit** |
| "Review my CLAUDE.md against best practices" | **audit** |
| "Claude keeps ignoring my rules" | **audit** (likely bloat) |
| "Explain how CLAUDE.md works" | **teach** |
| "I want to teach my students about CLAUDE.md" | **teach** |
| "What goes in CLAUDE.md vs docs/?" | **teach** (decision framework) |

If unclear, ask the user which mode they want by sharing the table.

## Workflow

### Step 1: Identify the Mode

If the user's request matches multiple modes, ask which they want. Common ambiguity: "set up CLAUDE.md for my project that I've been working on for months" could be **create** (start fresh) or **extract** (preserve session context). Ask.

### Step 2: Load the Mode-Specific Guide

Once mode is clear, read the corresponding file in `modes/`:

- `modes/create.md` — Creating CLAUDE.md from scratch for a new or existing project
- `modes/extract.md` — Extracting context from a long session into a CLAUDE.md system (6-phase Master Workflow)
- `modes/audit.md` — Reviewing/refactoring an existing CLAUDE.md
- `modes/teach.md` — Explaining concepts and giving learners templates

Each mode file contains the detailed workflow. Do not skip reading the mode file — it has specifics that differ between modes.

### Step 3: Use References as Needed

While executing the mode, load these as needed:

- `references/anti-patterns.md` — When auditing or warning user about bad patterns
- `references/decision-framework.md` — When user asks "should this go in CLAUDE.md?"
- `references/size-guidelines.md` — When sizing/pruning content
- `references/thai-language-notes.md` — When working with Thai users (mixed Thai/English, CLI bugs)

### Step 4: Use Templates as Starting Points

Templates live in `templates/`. Pick the right one and adapt — do NOT just dump a template into the user's project unchanged.

- `templates/minimal.md` — 30-line starter for beginners or simple projects
- `templates/standard.md` — 60-line balanced default
- `templates/monorepo.md` — Root + package overlay pattern
- `templates/non-code-content.md` — For creative writing, blog, marketing workspaces
- `templates/non-code-research.md` — For data science / research projects
- `templates/self-improving.md` — Includes meta-rules for compounding learning

### Step 5: Show Real Examples When Helpful

If the user asks "what does a good CLAUDE.md look like?" or you need to demonstrate a pattern, point them to `examples/`:

- `examples/README.md` — Index with one-line description of each
- Individual annotated examples covering Next.js, monorepo, data science, blog content, marketing

## Universal Rules for All Modes

### What Goes in CLAUDE.md

✅ **Include only if Claude can't infer it from code:**
- Project identity (1-line description + tech stack with versions)
- Bash commands (especially custom scripts like `pnpm test:watch:single`)
- Where things live (folder structure that's non-obvious)
- Conventions that differ from language/framework defaults
- Gotchas — real bugs that wasted real time
- Pointers to deeper docs

❌ **Do NOT include:**
- Things any linter/formatter enforces (indentation, semicolons, quotes)
- Generic best practices Claude already knows ("use meaningful variable names")
- Personality/role priming ("you are a senior engineer...")
- Long code samples (Claude already knows syntax)
- Things that change frequently (use docs/decisions.md or MEMORY.md instead)
- Secrets, API keys, credentials of any kind

### Sizing Rules

| Target | Use case |
|---|---|
| <30 lines | Simple project, minimal rules |
| 40–80 lines | **Default sweet spot** (HumanLayer, Boris Cherny's team) |
| 80–150 lines | Complex project; near attention limit |
| 150–200 lines | Anthropic's official ceiling |
| >200 lines | **Refactor — split into docs/ or skills** |

### Writing Style

- **Imperatives** > descriptions: "Use named exports" > "We prefer named exports"
- **Pair prohibitions with directions**: "Never use X; instead, do Y" — never just "Never X"
- **Bullets** for rules and lists; **prose only for WHY framing** that needs a sentence
- **Code blocks** only for literal commands or canonical examples — never long snippets
- **Use IMPORTANT** sparingly for genuinely critical rules (max 1-2 per file)

### Progressive Disclosure Setup

For projects beyond 80 lines of CLAUDE.md content, structure like this:

```
project-root/
├── CLAUDE.md              # 40-80 lines, orchestrator
├── CLAUDE.local.md        # .gitignored personal overrides
├── docs/
│   ├── architecture.md    # "Read when modifying core modules"
│   ├── decisions.md       # ADR-style log
│   └── gotchas-domain.md  # Domain-specific issues
└── .claude/
    ├── rules/             # Path-scoped rules with YAML frontmatter
    ├── commands/          # Slash commands (verbs)
    └── skills/            # On-demand skills
```

**Key distinction:**
- `@docs/file.md` syntax = loads at startup (saves nothing context-wise)
- Prose pointer like "read X when Y" = just-in-time, saves context
- For most cross-cutting concerns, prose pointer is preferred

## Output Quality Bar

After completing any mode, ALL of these must be true:

1. **Pruning test**: Every line passes "would removing it cause mistakes?"
2. **Pointer-not-content**: No long inline content; references docs instead
3. **Self-test**: Have Claude (or simulate) reading only the created files and explaining the project. If it can't, the files are insufficient.
4. **Versioning**: Suggest user commits to git so changes are tracked.
5. **No fabrication**: If unsure about something, mark `[uncertain]` and ask user — never invent.

## Communication Style

- Match the user's language (Thai/English/mixed)
- Be direct about what won't work — don't sugar-coat anti-patterns
- For Thai-language users, see `references/thai-language-notes.md` for known CLI bugs and mixed-language patterns
- Always explain WHY a recommendation matters, citing principle (attention limits, pruning test, etc.) — not just "best practice says so"

## When User Disagrees

Some users will push back on minimalism ("but I want to be thorough"). Respond:

1. Acknowledge their goal (thoroughness, safety, completeness)
2. Explain the actual failure mode: HumanLayer research shows that as instructions accumulate, LLM doesn't ignore *new* ones — it ignores *all* of them uniformly. More rules = fewer followed rules.
3. Offer a compromise: keep CLAUDE.md lean, move detail to `docs/` referenced by prose pointer. They get thoroughness AND adherence.

If they still want a 500-line CLAUDE.md, write it but include a clear note that this is against best practice and explain the tradeoff once. Then respect their decision.

## Key Sources (for citing when asked)

- **Anthropic official memory docs** — `https://code.claude.com/docs/en/memory` (canonical "target under 200 lines per CLAUDE.md file" rule) and `/best-practices` (canonical pruning-test phrasing)
- **HumanLayer "Writing a good CLAUDE.md"** by Kyle Mistele — `https://www.humanlayer.dev/blog/writing-a-good-claude-md`. Source of the "Claude is (not) an expensive linter" framing and the "instructions are ignored uniformly, not selectively" finding. HumanLayer's own CLAUDE.md is <60 lines; their explicit recommendation is <300 lines (shorter is better)
- **Vercel Engineering "AGENTS.md outperforms skills in our agent evals"** (Jan 27, 2026) — 53% baseline → 79% skills → 100% AGENTS.md docs index, tested on Next.js 16 APIs
- **Arize AI "CLAUDE.md best practices learned from optimizing Claude Code with prompt learning"** — +10% on SWE-Bench Lite when CLAUDE.md is optimized; methodology: split SWE-Bench Lite for train/test
- **ETH Zurich / LogicStar "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?"** by Gloaguen et al. (arXiv:2602.11988, ICLR 2026 workshop) — important counterweight: across 138 real Python tasks, AGENTS.md-style files tend to *reduce* success vs no context, with >20% cost increase. Mechanism: low-quality boilerplate dilutes attention from real code signals
- **"How Many Instructions Can LLMs Follow at Once?"** by Jaroslawicz et al. (arXiv:2507.11538) — IFScale benchmark; even frontier models max out at 68% accuracy at 500 instructions. Use this for the "more rules ≠ more followed rules" argument; don't claim a clean 150–200 threshold (the paper shows a gradient, not a cliff)
- **Boris Cherny** — Head of Claude Code at Anthropic, source of several production-pattern soundbites (Pragmatic Engineer interview, Lenny's Newsletter)

When citing to a user, be honest about provenance: Anthropic docs are first-party; HumanLayer/Vercel/Arize are community/practitioner; arXiv papers are peer-adjacent academic work. Quote numbers exactly when you have them, paraphrase when you don't, and offer to share the URL.
