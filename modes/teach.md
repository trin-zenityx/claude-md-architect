# Mode: Teach

Explain CLAUDE.md concepts to a learner and provide guided templates. Used both for one-on-one teaching and for generating educational content for students.

Use this mode when the user wants to:
- Understand how CLAUDE.md works conceptually
- Learn best practices before creating one
- Get a template with explanation suitable for teaching others
- Prepare educational content (course material, blog post, workshop)

## Workflow

### Phase 1: Calibrate the Learner

Ask one focused question to gauge level:

> "Quick question to tailor this for you: have you used Claude Code before, and have you written or used a CLAUDE.md file before?"

Map response to level:

| Response | Level |
|---|---|
| "Never used Claude Code" | Beginner |
| "Used Claude Code but not CLAUDE.md" | Intermediate |
| "Have a CLAUDE.md but want to learn more" | Intermediate |
| "Want to teach others" / "preparing course material" | Instructor (treat as advanced) |

For Instructors, ask: "Who's your audience? Their level?" then teach to that audience's level, not the instructor's.

### Phase 2: Teach in Layers

Build understanding in this order — do NOT jump ahead. Each layer must be confirmed understood before the next.

#### Layer 1: The "What" (1 minute)

```markdown
CLAUDE.md is a file that Claude Code reads automatically at the start
of every session in your project. It tells Claude things about your
project that aren't obvious from the code itself.

Think of it like onboarding a new engineer:
- Tech stack and versions
- How to run commands
- Where things live
- Things that broke before (gotchas)
- Rules your linter doesn't enforce
```

Confirm: "Make sense so far?"

#### Layer 2: The "Why It Matters" (2 minutes)

Explain attention budget:

```markdown
Every line in CLAUDE.md goes into Claude's context for EVERY message
you send in that project. So:

- 50 lines = small cost, Claude reads it every time, follows it well
- 500 lines = big cost, Claude reads it every time, follows it WORSE

Counter-intuitively, MORE rules = FEWER rules followed.

Research from HumanLayer found that as you add instructions, Claude
doesn't just ignore new ones — it starts ignoring all of them uniformly.
The academic IFScale benchmark (arXiv:2507.11538) shows even frontier
models drop to 68% accuracy at 500 instructions — and the slope down
starts much earlier, well before that.

So: less is more. Every line must earn its place.
```

Confirm understanding before next layer.

#### Layer 3: The Pruning Test (the key tool)

```markdown
There's ONE question that tells you if a line belongs in CLAUDE.md:

  "If I removed this line, would Claude make a mistake?"

- Yes → keep it
- Maybe → cut it
- No → definitely cut it

This is the single most important tool. Apply it to every line.

Examples:
- "Use 2-space indentation" → No. ESLint enforces this. CUT.
- "Run npm test before commit" → Yes. Claude can't guess your workflow. KEEP.
- "You are a senior engineer" → No. Personality has no effect. CUT.
- "Stripe webhook handler must validate signatures" → Yes. Not visible
  from filename alone. KEEP.
```

For beginners, give 3–5 more examples until they're predicting correctly.

#### Layer 4: Anatomy of a Good CLAUDE.md (5 minutes)

Show structure with example. Use `templates/standard.md` as the visual aid:

```markdown
A good CLAUDE.md has roughly this structure:

# Project Name              ← what it does, in one line
[Stack with versions]

## Commands                  ← bash commands Claude can't guess
- ...

## Structure                 ← where things live
- ...

## Rules                     ← conventions, paired prohibition+direction
- ...

## Gotchas                   ← real bugs that wasted time
- ...

## See also                  ← pointers to docs/ for deeper content
- ...
```

Walk through each section, explaining why it earns its place.

#### Layer 5: Progressive Disclosure (intermediate+ only)

For intermediates/instructors, teach the docs/ pattern:

```markdown
When you have too much to fit in 80 lines, split it:

CLAUDE.md           ← always loaded, lean (40–80 lines)
docs/
  architecture.md   ← read when modifying core (just-in-time)
  decisions.md      ← architectural decisions log
  gotchas-xyz.md    ← domain-specific issues

Key insight: pointing to docs from CLAUDE.md doesn't load them.
Claude reads them only when needed. This keeps the always-loaded
context lean while making knowledge available on demand.

Use:
- `@docs/file.md` → auto-loads at startup (NO context savings, just organization)
- Prose: "read docs/file.md when modifying X" → just-in-time, SAVES context
```

For beginners, skip this layer or mention briefly. They don't need it yet.

#### Layer 6: Anti-Patterns (5–10 minutes)

Show the top 5 anti-patterns with examples. Full list in `references/anti-patterns.md`.

```markdown
Top 5 things NOT to do:

1. Bloat — files >200 lines = Claude ignores rules
2. Code style rules — linter's job, not CLAUDE.md's
3. Personality priming — "you are a senior engineer" does nothing useful
4. Negation-only — "Never use X" without saying "do Y instead" gets ignored
5. Auto-generated /init output committed unchanged — usually mostly noise
```

For each, show a "bad" example then a "good" replacement.

### Phase 3: Hands-On Practice (optional but recommended)

For learners who want to practice:

> "Want to try writing a CLAUDE.md for one of your projects right now?"

If yes → switch to `create` mode with the learner. They write, you guide. Apply the pruning test together in real time.

For instructors generating course material:

> "Want me to generate practice exercises for your students?"

If yes, generate a set like:

```markdown
## Exercise 1: Spot the Anti-Pattern
[Show bad CLAUDE.md, ask student to identify issues]

## Exercise 2: Apply the Pruning Test
[Show 10 lines, ask which to keep]

## Exercise 3: Refactor This
[Show 200-line file, ask student to refactor to 50 lines]

## Exercise 4: Write One From Scratch
[Give project spec, ask student to write CLAUDE.md]
```

### Phase 4: Closing & Resources

End with:

```markdown
## Key takeaways to remember
1. Less is more — under 80 lines is a good target
2. Pruning test on every line
3. Use docs/ for content that's not needed every session
4. Refactor periodically (every quarter is healthy)

## To go deeper
- Official docs: code.claude.com/docs/en/memory
- HumanLayer's guide (the canonical community guide)
- This skill's references/ folder has detailed anti-patterns

## To start applying
- Try `create` mode for a new project
- Try `audit` mode on a CLAUDE.md you already have
- Try `extract` mode if you have a long session to preserve
```

## Variations

### Variation: Course Material Generation

If the user is an instructor preparing course material (e.g., "ผมจะเอาไปสอนนักเรียน ZenityX"):

Adjust outputs:

1. **Generate slides outline** with key bullet points per layer
2. **Generate exercises** with answer keys
3. **Generate cheat sheets** (1-page reference for students)
4. **Generate a graded difficulty progression** for assignments
5. **Highlight Thai-language considerations** (see `references/thai-language-notes.md`)

Suggest structure for a workshop:

```markdown
## Suggested workshop structure (90 min)

10 min — Layer 1+2: What & Why
15 min — Layer 3: Pruning Test (with examples)
20 min — Layer 4: Anatomy + walk through template
15 min — Layer 6: Anti-patterns (with bad examples)
25 min — Hands-on: students write CLAUDE.md for their project
 5 min — Closing & resources

(Skip Layer 5 for beginners; add it for intermediate workshops)
```

### Variation: Thai Audience

If teaching in Thai or to Thai students:

- Match user's language (mostly Thai with English technical terms)
- Reference `references/thai-language-notes.md` for known CLI bugs
- Use the pattern: "CLAUDE.md (อ่านว่า 'คลอด เอ็ม ดี')" first mention
- Keep technical terms in English (Tokens, attention, pruning test) — students need to recognize them in docs

### Variation: Quick Explanation (no full lesson wanted)

If user just wants a brief explanation, not a course:

Skip layered teaching. Give a 1-paragraph summary + 1 example + pointer to learn more.

```markdown
CLAUDE.md is a file Claude Code reads at every session start. It contains
project context: stack, commands, conventions, gotchas. Best kept under 80
lines — bloat causes Claude to ignore rules. Each line should pass: "if I
removed this, would Claude make a mistake?"

Want a deeper explanation, a template, or help creating one?
```

### Variation: Comparing to Other AI Coding Tools

If user mentions Cursor, Copilot, Cline, Continue, etc.:

```markdown
Most AI coding tools have a similar concept:
- Claude Code: CLAUDE.md
- Cursor: .cursorrules or .cursor/rules
- GitHub Copilot: .github/copilot-instructions.md
- AGENTS.md: emerging cross-tool standard
- Continue: config.json with system prompts

The principles are largely the same: be specific, keep it lean, document
non-obvious things. CLAUDE.md content can often migrate to/from these.

Anthropic uses both CLAUDE.md and AGENTS.md format in their own
documentation now — they're effectively interchangeable.
```

## Teaching Anti-Patterns

❌ **Teaching all 6 layers to beginners** — overwhelm. Layers 1–4 is enough; layer 5 (progressive disclosure) is intermediate, layer 6 (anti-patterns) is best after they've tried writing one.

❌ **Showing only good examples** — students learn faster from "bad vs good" pairs

❌ **Skipping the pruning test** — this is THE key tool. Spend time on it.

❌ **Jumping to advanced patterns** (subdirectory CLAUDE.md, path-scoped rules, skills) before they understand basics

❌ **Reading the slide deck at them** — interactive: ask them to predict, then reveal

❌ **Ignoring their actual project** — generic teaching is fine, but immediately applying to their work makes it stick

## Quality Checklist Before Finishing

- [ ] User's level was calibrated in Phase 1
- [ ] Teaching matched their level (not too advanced, not too basic)
- [ ] Pruning test was thoroughly demonstrated with examples
- [ ] User can predict at least 5 anti-patterns from examples
- [ ] User knows next concrete step (try create mode, audit existing, etc.)
- [ ] References provided for going deeper
