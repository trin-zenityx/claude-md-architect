# Mode: Audit

Review an existing CLAUDE.md (and supporting docs) against best practices, identify issues, and propose refactoring.

Use this mode when the user has an existing CLAUDE.md that they suspect is bloated, ineffective, or out of date. Common symptoms: Claude ignoring rules, file >150 lines, file unchanged for months, rules contradict each other.

## Workflow

### Phase 1: Read Everything

Before forming opinions, read the entire current setup:

1. `CLAUDE.md` (root)
2. `CLAUDE.local.md` if exists (note: may be .gitignored, ask user to share)
3. Any `~/.claude/CLAUDE.md` (user-level, if relevant)
4. Files in `docs/` referenced from CLAUDE.md
5. Files in `.claude/rules/` if present
6. Any subdirectory `CLAUDE.md` files (`src/api/CLAUDE.md`, etc.)
7. `.claude/skills/` and `.claude/commands/` if relevant

Count total lines. Note the structure.

### Phase 2: Diagnostic Pass

Apply the following checks. For each, mark ✅ passing or ❌ issue:

#### A. Size Check

| Check | Threshold | Status |
|---|---|---|
| Root CLAUDE.md ≤80 lines | 80 lines (sweet spot) | |
| Root CLAUDE.md ≤200 lines | 200 lines (Anthropic ceiling) | |
| Total context budget reasonable | Sum of all auto-loaded files | |

#### B. Pruning Test

Go through each line of CLAUDE.md. For each, ask: **"Would removing this cause Claude to make mistakes?"**

If "no" or "probably not" → flag for cutting.

Report findings as:

```markdown
### Lines that could be cut:
- Line 12 ("Use camelCase variables"): ESLint enforces this
- Line 15 ("Write good code"): generic, Claude knows
- Lines 23–27 (full code snippet): Claude knows syntax, points to file instead
- Line 34 ("You are a senior engineer..."): personality bloat
```

#### C. Anti-Pattern Check

Scan for the 10 documented anti-patterns (full details in `references/anti-patterns.md`):

1. **Bloated catch-all** (500+ lines, kitchen sink)
2. **Claude as linter** (formatting rules)
3. **Auto-generated and untouched** (looks like `/init` output, never edited)
4. **Negation-only constraints** ("Never X" without "do Y instead")
5. **Personality bloat** ("you are a senior...")
6. **Stale and contradictory** (rules conflict with each other or code)
7. **Conflicting tool instructions** (user vs project CLAUDE.md disagree)
8. **One rule per mistake** (300-line gotchas section)
9. **Importing everything via @** (huge context blowout)
10. **CLAUDE.md as documentation** (treats it as project README)

Report which patterns are present.

#### D. Structural Check

- Does CLAUDE.md follow a clear structure (Why / What / How / See also)?
- Are rules paired (prohibition + direction)?
- Are commands listed clearly?
- Are gotchas grouped logically?
- Is there a clear "See also" or pointer section?

#### E. Reference Integrity Check

For any `@docs/...` imports or prose pointers:

- Does the referenced file exist?
- Is it actually being used (or is it dead reference)?
- Is the content in the right place (in CLAUDE.md vs docs/)?

#### F. Currency Check

Read git log on CLAUDE.md if possible:

```bash
git log --follow --oneline CLAUDE.md
```

- Is it being updated regularly?
- Last update vs last code change — if huge gap, file is likely stale
- Are there rules referencing modules/folders that no longer exist?

### Phase 3: Severity Assessment

Categorize issues found:

**🚨 Critical** (likely causing Claude to ignore rules):
- Bloat over 200 lines
- Contradictory rules
- Stale rules pointing to non-existent code

**⚠️ Important** (degrading quality):
- 30–50% of lines fail pruning test
- Missing structure
- Negation-only constraints

**ℹ️ Minor** (polish):
- Could be more concise
- Missing helpful pointers
- Inconsistent formatting

### Phase 4: Refactor Plan

Show the user a refactor plan before making changes:

```markdown
## Current State
- CLAUDE.md: 247 lines
- docs/: 3 files (2 referenced, 1 orphaned)
- Issues found: 4 critical, 6 important, 3 minor

## Proposed Refactor

### CLAUDE.md (will become ~65 lines)
**Keep:**
- Lines 1–8 (project identity)
- Lines 18–23 (commands)
- Lines 45–58 (top gotchas)

**Cut:**
- Lines 12–17 (formatting — moves to nothing, linter handles)
- Lines 24–44 (architecture deep-dive — moves to docs/architecture.md)
- Lines 89–120 (old gotchas about deprecated module — delete entirely)
- Lines 130–247 (long examples and explanations — moves to docs/)

### New docs/ files
- `docs/architecture.md` (extracted from current lines 24–44)
- `docs/gotchas-detailed.md` (extracted, only for non-obvious items)

### Files to delete
- `docs/old-notes.md` (orphaned, no references)

### Quantified impact
- Context per session reduced by ~70%
- Attention budget freed up: ~5% of context window

Approve refactor? Make changes one section at a time?
```

Wait for user approval. They may want to keep some things you'd cut.

### Phase 5: Execute Refactor

If user approves:

1. **Backup first**: suggest `git commit` of current state before editing
2. **Refactor incrementally**:
   - First pass: write new CLAUDE.md
   - Second pass: create new docs/ files with extracted content
   - Third pass: delete orphaned files
3. **Verify references**: ensure every `@import` and prose pointer in new CLAUDE.md resolves
4. **Use `Edit` for targeted edits**, not full rewrites where possible (preserves git diff value)

### Phase 6: Self-Test

After refactor, simulate a fresh session reading only the new files:

> "If I were a new Claude session, having read only the refactored files, would I:
> - Understand what the project does?
> - Know the most critical rules?
> - Know where to look for deeper info?
> - Not be confused by missing things?"

State this back to user. If gaps → iterate.

### Phase 7: Migration Notes

Tell the user what changed in plain language:

```markdown
## What changed
- CLAUDE.md went from 247 → 64 lines (74% reduction)
- 3 anti-patterns removed
- 2 new docs/ files created
- 1 orphaned doc deleted

## What to watch for in next sessions
- If Claude does X (specific thing the cut rules used to enforce),
  it may need a reminder once. Add it back if it recurs.
- The architecture deep-dive moved to docs/architecture.md.
  Use "@docs/architecture.md" if it needs auto-loading,
  or just reference it in prose for on-demand reading.

## Suggested commit
git add CLAUDE.md docs/
git rm docs/old-notes.md
git commit -m "Refactor CLAUDE.md: 247 → 64 lines, remove anti-patterns"
```

## Variations

### Variation: Audit Without Permission to Change

User might want a diagnosis but not authorize changes. Stop after Phase 3. Provide written diagnostic report with recommendations. Note: this is also good for teaching — `teach` mode borrows this output format.

### Variation: Multi-File System Audit

If project has CLAUDE.md + subdirectory CLAUDE.md files + .claude/rules/:

In Phase 1, map the entire system. In Phase 2, additionally check:

- **Inheritance conflicts**: does subdirectory CLAUDE.md contradict root?
- **Coverage gaps**: are there subdirectories that should have their own CLAUDE.md but don't?
- **Path-scoped rules**: are .claude/rules/*.md files using proper YAML `paths:` frontmatter?

### Variation: Team-Owned CLAUDE.md

If multiple people contribute:

In Phase 6 (migration notes), add:

```markdown
## Communication to team
Suggested message:
"I refactored CLAUDE.md to be more focused. Removed [X] anti-patterns.
Long content moved to docs/. Please review and flag if anything you
needed was removed. The principle: every line in CLAUDE.md should
prevent a specific kind of mistake. If it doesn't, it dilutes attention
on the rules that do matter."
```

### Variation: Audit for Production / Pre-Release

If the project is going to production or is being shared widely:

Extra checks in Phase 2:
- **Secret leakage**: any credentials, API endpoints, internal URLs?
- **Personal info**: names, emails, internal team references?
- **Stale public info**: outdated version numbers, removed features?

These items must go before going public regardless of "is it helpful for Claude".

## Common Issues You Will Find

In auditing real-world CLAUDE.md files, the most frequent issues (in order):

1. **Bloat from never deleting anything** — files only grow. Most rules added over time should have been deleted long ago.
2. **Code style rules a linter handles** — easy to spot, easy to remove.
3. **Generic advice Claude already knows** — "use meaningful names", "write tests", etc.
4. **Long code samples** — usually pasted in early then never updated; now they're wrong.
5. **Negation-only rules** — "Never use class components" without saying what to use.

Most refactors cut 50–70% of the file. Don't be shy about cutting.

## Anti-Patterns in Audit Mode

❌ **Cutting without explaining** — user needs to understand WHY each cut is correct, both to approve and to learn for next time

❌ **Refactoring before reading everything** — easy to miss connections, break references

❌ **Doing whole-file rewrite when surgical edits work** — preserves git history value

❌ **Skipping self-test** — refactor that breaks understanding is worse than bloat

❌ **Not backing up first** — `git commit` before edit gives safe rollback

## Quality Checklist Before Finishing

- [ ] All files in the system were read in Phase 1
- [ ] All 10 anti-patterns were explicitly checked
- [ ] Refactor plan was approved by user before execution
- [ ] User committed pre-refactor state (rollback safety)
- [ ] All references in new files resolve (no dead `@imports`)
- [ ] Phase 6 self-test passed
- [ ] Migration notes explained for user
- [ ] Final CLAUDE.md is ≤80 lines (or ≤200 with strong justification)
