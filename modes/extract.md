# Mode: Extract

Extract context from a long, in-progress Claude session into a CLAUDE.md system, so a future session can continue the work seamlessly.

Use this mode when the user has been working with Claude on a project for an extended time, hasn't yet created a CLAUDE.md, and wants to preserve the accumulated context before closing the session.

This is high-stakes work: the session's chat history is gold (decisions made, bugs encountered, dead ends explored) but it's about to evaporate. The extraction must be thorough AND honest about what's been said vs. fabricated.

## The 6-Phase Master Workflow

### Phase 0: Pre-flight Safety Check

Before starting extraction, recommend the user:

1. **Fork the session** (in Claude Code Desktop, click Fork) — work in a copy, not the original
2. **Create a git branch** if the project uses git:
   ```bash
   git checkout -b experiment/claude-md-setup
   ```
3. **Confirm working tree is clean** with `git status` — extraction will create files

If user declines, proceed but note the risk: if the extraction goes wrong, session is harder to recover.

### Phase 1: Memory Dump (no file writing yet)

Scan the entire conversation history and dump everything relevant into a structured list. Output to chat — do NOT write files yet.

Format:

```markdown
## Discovered Context

### A. Project Identity
- Name: ...
- What it does: ...
- Stack: ... (versions if mentioned)
- Status: (active development / maintenance / one-off)

### B. Decisions Made
- [date or session reference] Decision: ... | Why: ... | Alternatives rejected: ...
- ...

### C. Gotchas Encountered
- ... (bug or quirk + how resolved + lesson)

### D. Current Status
- Completed: ...
- In progress: ... (state exactly where it stopped)
- Blocked on: ...
- Next planned: ...

### E. Conventions / Style
- Naming: ...
- Patterns: ...
- Test approach: ...

### F. User Context
- Working style preferences: ...
- Jargon used: ...
- People mentioned: ...

### G. Failed Attempts
- Tried: ... | Failed because: ... | Should not retry

### H. Open Questions
- Things left undecided: ...
```

**CRITICAL RULES for Phase 1:**

- **No fabrication.** If you're unsure whether something was discussed, mark `[uncertain — verify with user]`
- **Partial recall**: if you remember something fuzzy, mark `[partial recall]` and quote what you have
- **No interpretation beyond what was said** — extract, don't extrapolate
- **Raw list format, not pretty prose** — Phase 4 makes it pretty; Phase 1 makes it complete

Stop after dumping. Wait for user to review.

### Phase 2: Verification & Augmentation

The user reviews Phase 1 output. Their job:
- Confirm what's correct
- Mark `[uncertain]` items as confirmed/wrong
- Add things you missed
- Delete things that aren't relevant

After their review, ask follow-up questions in batches of 2–3:

**Always ask:**
1. "Anything you decided to do but never told me explicitly?"
2. "Anything about stakeholders, users, or business context I should know?"
3. "Production URLs, environments, secrets that exist (just the variable names, not values)?"

**Ask if relevant from Phase 1 gaps:**
4. "I see we discussed X but never resolved Y — what's the decision now?"
5. "What's the source of truth for [thing that was ambiguous]?"

Use `AskUserQuestion` if available. Wait for answers before Phase 3.

### Phase 3: Triage & Structure

Now classify each item from Phase 1+2 into a destination. Show the mapping to the user before writing.

```markdown
## File Mapping (please approve before writing)

### → CLAUDE.md (universal context, always loaded)
- Project identity
- Stack
- Commands
- Top 5–10 most-applied rules
- Pointers to other files

### → docs/decisions.md
- Decision A (date, what, why)
- Decision B
- ...

### → docs/gotchas-[domain].md
- Gotcha 1
- Gotcha 2
- ...

### → progress.md (session handoff)
- Current status
- Next steps
- Open questions

### → CLAUDE.local.md (personal, .gitignored)
- User's preferred working style
- Personal shortcuts

### → DROPPED (not worth recording)
- Trivial details
- One-time conversations not relevant to future sessions
- Things visible in code
```

Wait for user approval. They may move items between buckets.

### Phase 4: Write Files

Write the approved files. Order:

1. `CLAUDE.md` (root)
2. `docs/*.md` files referenced from CLAUDE.md
3. `progress.md` (session handoff doc)
4. `CLAUDE.local.md` if any personal items
5. `HANDOFF.md` (temporary, for Phase 6 — see below)

Apply size rules:
- CLAUDE.md ≤80 lines
- docs/ files no hard limit but stay focused per file (one topic per file)
- progress.md: current state + next steps, keep concise

Use pointers, not content. CLAUDE.md should `@docs/decisions.md` only if decisions need to load every session; otherwise prose pointer.

### Phase 5: Self-Test (CRITICAL)

Simulate being a fresh Claude session that has never seen this conversation. Read ONLY the just-created files. Then answer in chat:

1. "What does this project do?"
2. "What's the current status?"
3. "If user asked me to continue working on [last in-progress item], would I know enough?"
4. "What anti-patterns/gotchas should I avoid?"
5. "Is anything obviously missing?"

Report this self-test honestly to the user. If gaps exist, go back to Phase 4 and add them.

This is the single most important quality gate. The whole point of extraction is enabling future sessions — if Phase 5 fails, the extraction failed.

### Phase 6: Handoff Note

Create `HANDOFF.md` in the project root (temporary, can delete after first new session):

```markdown
# Session Handoff

**Date**: [today]
**Extracted from**: session on [topic]
**Files created**: CLAUDE.md, docs/..., progress.md

## Starter prompt for next session

Paste this into a new Claude session in this directory:

> Read CLAUDE.md and progress.md to understand context. Then summarize:
> 1. What the project is
> 2. Current status
> 3. Next planned task
> Wait for me to confirm before proceeding.

## Verify in first new session

These items were marked [uncertain] or [partial recall] — confirm with user early:
- [item 1]
- [item 2]

## Next immediate task

[the very next thing to work on]
```

Then commit:

```bash
git add CLAUDE.md docs/ progress.md HANDOFF.md
git commit -m "Initial context system extracted from session"
```

User can delete HANDOFF.md after the first successful new session.

## Variation: Session With Multiple Topics

If the long session covered multiple unrelated topics (project A, debug B, brainstorm C):

Insert **Phase 0.5** before Phase 1:

```markdown
## Topic Segmentation
List all distinct topics in this session:
1. [topic] — about [project] — relevant to extraction: yes/no
2. ...
```

Have user mark which topics are relevant. Phase 1+ only extracts marked topics.

## Variation: Session Heavy on Code (Already Implemented)

If session involved Claude writing lots of code that user already integrated:

In Phase 1.I, add:

```markdown
### I. Code Generated & Status
- Function/file X: implemented in lib/foo.ts
- Function Y: partially implemented, completion pending
- Approach Z: rejected — explain why in failed attempts
```

In Phase 4, CLAUDE.md should NOT include code snippets — use pointers: "see lib/foo.ts for the X pattern."

## Variation: Brainstorm/Strategy Session (Non-Code)

If session was strategic/planning with no codebase yet:

Output structure shifts:
- `STRATEGY.md` instead of `CLAUDE.md` as primary
- `decisions.md` with rationale-heavy entries
- `ideas-parking-lot.md` for ideas discussed but not chosen
- CLAUDE.md becomes small, links to STRATEGY.md

In Phase 1, emphasize:
- B. Decisions Made → biggest section
- G. Failed Attempts → "ideas considered and rejected"
- H. Open Questions → "what we still need to figure out"

## Variation: Session With Sensitive Information

If session discussed credentials, customer names, financial data:

Insert **Phase 0.5: Sanitization Rules**:

```markdown
## Things that will NEVER appear in extracted files:
- API keys, tokens, passwords (real values)
- Customer real names (use [Customer A] etc.)
- Internal pricing/financials
- Access credentials

## Placeholder strategy:
- "API key for X (stored in .env as X_API_KEY)"
- "Customer A (insurance industry)"
- "Internal pricing — see ops doc, not here"
```

Apply throughout extraction.

## Variation: Session Covering Multiple Projects

If user has been working on 2–3 projects in one session:

Repeat the entire 6-phase workflow once per project. Do not try to share files across projects. Each project gets its own CLAUDE.md.

After all are done, in **Phase 6.5**, identify cross-project knowledge:

> "I noticed [pattern X] appeared in both projects. Want me to extract it as a shared skill at `~/.claude/skills/` or as a shared docs/ file?"

## Common Pitfalls in Extract Mode

❌ **Letting Claude fabricate to fill gaps** — if memory is fuzzy, mark `[uncertain]`, don't invent

❌ **Writing files before user verification (Phase 2)** — they always have context you missed

❌ **Including session-only artifacts** (debug conversations, jokes, off-topic) — extract for the future, not as transcript

❌ **Making CLAUDE.md a session summary** — it should be project context for future sessions, not "what we did today"

❌ **Skipping Phase 5 self-test** — extraction quality is only verifiable via simulation

❌ **Trying to extract while the session is also continuing work** — pause work, extract, then decide whether to continue or close. Mixing causes mess.

## Quality Checklist Before Finishing

- [ ] Phase 1 included `[uncertain]` markers where appropriate
- [ ] Phase 2 verification was done (user reviewed before file writing)
- [ ] Phase 3 mapping was approved by user
- [ ] Phase 5 self-test passed (new-session simulation made sense)
- [ ] HANDOFF.md created with starter prompt
- [ ] User reminded to commit and to verify [uncertain] items in next session
- [ ] CLAUDE.md ≤80 lines
- [ ] No fabricated information

## Time Estimate

For session of 50–100 messages, expect:
- Phase 1: 5–10 min (Claude work)
- Phase 2: 15–30 min (user review — longest phase)
- Phase 3: 5 min
- Phase 4: 10–15 min
- Phase 5: 10 min
- Phase 6: 5 min

**Total: 60–90 min**

If longer than that, scope is probably too big — break into separate projects via the multi-project variation.
