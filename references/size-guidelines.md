# Size Guidelines Reference

Quantitative guidelines for sizing CLAUDE.md files and supporting documents, based on evidence from Anthropic official docs and production teams.

## Quick Reference Table

| File | Target | Hard Ceiling | When to refactor |
|---|---|---|---|
| Root CLAUDE.md | 40–80 lines | 200 lines | >150 lines |
| User CLAUDE.md (~/.claude/) | 5–15 lines | 50 lines | >30 lines |
| Subdirectory CLAUDE.md | 20–40 lines | 100 lines | >60 lines |
| CLAUDE.local.md | <30 lines | 80 lines | >50 lines |
| docs/*.md (per topic) | 50–200 lines | 500 lines | >400 lines |
| progress.md | <40 lines | 100 lines | >80 lines |

## Sources for These Numbers

| Source | Recommendation |
|---|---|
| Anthropic official memory docs ([code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)) | "Target under 200 lines per CLAUDE.md file" (canonical) |
| HumanLayer "Writing a good CLAUDE.md" by Kyle Mistele | Explicit: "<300 lines is best, shorter is even better". Their own file is <60 lines |
| Boris Cherny (Head of Claude Code, Anthropic) | Vanilla minimal setup |
| Community practitioner guides (TurboDocx, Blink, etc.) | Ideal 50–100, ceiling 300 |
| Community templates (e.g. abhishekray07/claude-md-templates) | "If over 80 lines, Claude starts ignoring parts" |
| Jaroslawicz et al., IFScale (arXiv:2507.11538) | Frontier models max at 68% accuracy at 500 instructions — gradient, not a clean threshold |
| Gloaguen et al., ETH Zurich AGENTS.md eval (arXiv:2602.11988, ICLR 2026 workshop) | Low-quality context files actively reduce success rates; >20% cost increase |

## The Math: Why These Numbers

### Token Cost

- Average line of CLAUDE.md: 8–15 tokens
- 80-line file: ~800–1,200 tokens
- 200-line file: ~2,000–3,000 tokens
- Claude Opus 4.7 context: 200K tokens

200 lines = ~1.5% of context window. **The real cost isn't tokens — it's attention dilution.**

### Attention Limit

LLM instruction-following degrades **gradually** with density. The IFScale benchmark (Jaroslawicz et al., arXiv:2507.11538) shows even frontier models hit only 68% accuracy at 500 instructions, with steady decline before that. Don't expect a clean 150-instruction cliff — expect a slope.

What HumanLayer observed in practice: the model doesn't selectively ignore — once you cross a fuzzy budget, it ignores rules **uniformly**. More rules = fewer rules followed. Claude Code's system prompt already consumes context before you add a line.

### Why "80 lines" Specifically?

80 lines is the experiential sweet spot reported by production teams:
- Large enough to capture meaningful project context
- Small enough that Claude reads it carefully every time
- Forces healthy discipline ("does this line really earn its place?")
- Most projects' essential context fits in 80 lines if you're honest

Going over 80 doesn't automatically break Claude — but it requires *justification*. By 150 you're approaching the ceiling. Past 200 is provably harmful per the ETH Zurich study.

## Sizing by Section

### Typical 60-line CLAUDE.md breakdown

```
Section          | Lines | Notes
-----------------|-------|---------------------------------
Title + identity |   3   | 1-line description + stack
(blank)          |   1   |
## Commands      |   8   | 5–8 essential commands
(blank)          |   1   |
## Structure     |   5   | 3–5 key folders
(blank)          |   1   |
## Rules         |  15   | 5–10 paired rules
(blank)          |   1   |
## Gotchas       |  10   | 3–8 real issues
(blank)          |   1   |
## See also      |   5   | 3–5 pointers
                 |-------|
Total            |  ~55  |
```

Common ranges per section:

| Section | Min | Max | Median |
|---|---|---|---|
| Title + identity | 2 | 5 | 3 |
| Commands | 3 | 12 | 8 |
| Structure | 3 | 10 | 5 |
| Rules | 5 | 20 | 12 |
| Gotchas | 0 | 15 | 8 |
| See also | 3 | 10 | 5 |

Sum: 16–72 lines, median ~41 + section spacing → typical 50–60 lines.

## When to Split

### Split CLAUDE.md when:

- File approaches 150 lines
- A single section dominates (e.g., gotchas section is 80 lines)
- Different team members own different sections
- Content has clear domain boundaries (Stripe stuff, auth stuff, etc.)

### How to split:

**Option A**: Move detail to `docs/<topic>.md`, reference by prose pointer

Before:
```markdown
## Stripe Integration

[40 lines of Stripe-specific gotchas, patterns, etc.]
```

After:
```markdown
## See also
- For Stripe integration issues, read `docs/stripe-guide.md`
```

**Option B**: Move to subdirectory CLAUDE.md

Before (in root CLAUDE.md):
```markdown
## API Layer
[30 lines about src/api/ specifically]
```

After:
- Root CLAUDE.md: remove those lines
- `src/api/CLAUDE.md`: created with those 30 lines, plus its own context

The subdirectory file loads only when Claude touches files in `src/api/`.

**Option C**: Promote to skill

If content is reusable across projects (e.g., "how we test LINE Bot integrations"), move to `~/.claude/skills/line-bot-testing/SKILL.md`.

## When Smaller Is Better

Sometimes 30 lines is better than 60. When:

- Very simple project (one script, one tutorial, etc.)
- Single concern, no complexity to capture
- Beginner is learning — start minimal, grow as needed
- Solo dev, comfortable codebase — most things are inferable

Karpathy's famous 4-rule file is an example of useful minimalism.

## When Larger Is Justified

Up to 150 lines is justified when:

- Production codebase with many subtle conventions
- Lots of domain-specific gotchas (regulated industry, complex API integrations)
- Team project where conventions must be enforced
- Migration in progress (old + new patterns coexist temporarily)

Past 150 lines, you should be moving content out, not adding more.

## Per-File Size Logic

### Root CLAUDE.md: 40–80 lines

Always loaded. Highest stakes for attention. Smallest target.

### `~/.claude/CLAUDE.md` (user level): 5–15 lines

Applies to every project Claude works on for you. Keep it to truly universal preferences. Often just 5–10 lines.

Example:
```markdown
# Personal Preferences

- Respond in concise prose; bullets only when listing
- When uncertain, ask one targeted question — don't guess
- For code changes, show me a diff summary before applying
- I run macOS; commands assume zsh
```

### Subdirectory CLAUDE.md: 20–40 lines

Only loads when working in that directory. Should focus on rules specific to that area, not repeat root rules.

### CLAUDE.local.md: <30 lines

Personal, .gitignored. Things like:
- Your specific dev DB URL
- Your local file paths
- Your preferred shortcuts
- Notes from earlier sessions you want to remember

### docs/<topic>.md: 50–200 lines

Loaded on-demand. Can be larger because not every session pays the cost.

Per-file rule: **one topic per file**. If `docs/architecture.md` is becoming a monster, split into `architecture-frontend.md`, `architecture-backend.md`, `architecture-data.md`.

### progress.md: <40 lines

Should fit a single screen. Current status, next steps, blockers. Update frequently.

Format:
```markdown
# Progress

## Done
- ...

## In Progress
- ... (note exactly where it stopped)

## Next
- ...

## Blocked
- ... (and on what)
```

## Anti-Pattern: Lines That Don't Count

These don't count toward your size budget, but check them anyway:

- HTML comments (`<!-- ... -->`) — stripped before injection
- Comments inside code blocks — preserved
- Code blocks themselves — counted, expensive
- Tables — counted, expensive per row

If you're using many large tables in CLAUDE.md, you're likely doing it wrong. Tables are for reference docs, not always-loaded context.

## Detecting Bloat

Quick diagnostic:

1. **Open CLAUDE.md**
2. **Run pruning test on each line**: "if removed, would Claude make a mistake?"
3. **Count "no" or "maybe" answers**

| % "no/maybe" | Diagnosis |
|---|---|
| 0–10% | Healthy |
| 10–30% | Light maintenance needed |
| 30–60% | Significant refactor needed |
| 60%+ | Bloat — major refactor |

## Refactor Frequency

| Frequency | When |
|---|---|
| Continuous | Add lines as you observe mistakes; remove lines you notice are stale |
| Quarterly | Full pruning pass |
| After major refactor | Update sections relating to changed code |
| Before new contributor onboards | Make sure it represents current state |

## Comparing Across Languages (Thai/English)

Thai characters use 2–4× more UTF-8 bytes than English ASCII. Token counts for Thai content can be similarly higher.

Implications:
- A 60-line Thai CLAUDE.md has more tokens than 60-line English
- The attention limit applies to *number of instructions*, not bytes — so 60 Thai instructions is still ~60 instructions
- Token cost is higher; budget impact is real but small (~1.5% → ~3%)

Recommendation: Same line targets apply regardless of language. Don't compensate by writing fewer Thai rules — write the right number, just expect slightly higher token usage.
