# [Project Name]
[One-sentence purpose]

Stack: [...]

## Meta-rules (read first)

When you make a mistake and I correct you:
1. **Reflect**: What was the root cause?
2. **Abstract**: What general pattern would prevent this in the future?
3. **Add**: Append a one-line entry to `## Gotchas` below, dated, if the rule would prevent future mistakes
4. **Keep entries under 2 lines each**
5. **Promote**: If the same kind of mistake appears 2+ times, move the rule from `## Gotchas` up to `## Rules`
6. **Quarterly**: I will prune entries that no longer apply; flag any that seem stale

When you encounter a situation not covered:
- Ask one targeted question rather than guessing
- After resolution, propose adding a rule (don't add unilaterally)

## Commands
- [your commands here]

## Structure
- [your structure here]

## Rules
<!-- Promoted from Gotchas after 2+ recurrences -->
- [stable rule 1]
- [stable rule 2]

## Gotchas
<!-- New learnings accumulate here, dated. Format: YYYY-MM-DD: [issue + fix] -->
- [YYYY-MM-DD]: [issue + how to handle]
- [YYYY-MM-DD]: [issue + how to handle]

## See also
- `MEMORY.md` — auto memory written by Claude (don't edit manually)
- `docs/decisions/` — ADRs for major architectural choices
- `progress.md` — current sprint status

---

## How to use this template

This is for projects where you want CLAUDE.md to grow with you. The meta-rules teach Claude HOW to maintain the file, not just what's in it.

**Pros**:
- File evolves naturally with project knowledge
- Claude proposes additions, you approve
- Quarterly prune keeps it lean

**Cons**:
- Slightly longer base (meta-rules section)
- Requires you to actually approve/reject proposed additions
- Easy to let Gotchas section bloat if you skip pruning

**Best for**: solo developers, side projects, learning projects where you want to build the muscle of CLAUDE.md maintenance.

**Not great for**: team projects (the meta-rules can conflict with team conventions on who edits CLAUDE.md), strict production codebases (use the standard template).
