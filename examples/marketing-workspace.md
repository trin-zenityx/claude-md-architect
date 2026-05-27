# Example: Marketing Workspace (Non-Code)

Shows CLAUDE.md adapted for a non-engineering role. "About me" + workflow rules turn Claude Code into an orchestration tool for marketing work.

## The File

```markdown
# Acme Marketing — Sarah's Workspace
B2B SaaS marketing team workspace. I run quarterly campaign cycles.

## About me
- Sarah Chen, Marketing Lead at Acme
- I lead campaign strategy and own brand voice
- Team: Lisa (Content), Raj (Paid Ads), Maya (Designer)

## Layout
- `campaigns/Q1-2026/` — current quarter's work
- `campaigns/Q4-2025/` — previous quarter (archived but accessible)
- `brand/` — voice guide, visual identity, messaging frameworks
- `research/` — competitor analysis, audience research, market data
- `templates/` — reusable structures for emails, landing pages, ads

## Target Audience
- Primary: B2B SaaS Product Managers, 30-45, technically literate, time-poor
- Secondary: Engineering managers evaluating tools for their team
- Tone register: Professional but warm. Confident but not arrogant.

## Voice
- On-brand: warm, specific, evidence-based
- Never use: synergy, innovative, leverage (as verb), best-in-class, robust,
  cutting-edge, seamless, holistic
- Always prefer: specific numbers over adjectives ("3x faster" not "much faster")
- We write to one person, not to "users" or "customers" in aggregate

## Workflow Rules
- When I say "Lisa thinks X", treat as approved direction
- When I say "Raj said the data shows X", treat as factual
- Drafts go in `campaigns/<quarter>/drafts/`
- Finals go in `campaigns/<quarter>/approved/` after my sign-off
- Always propose 2-3 options for headlines, taglines, and CTAs
- For social posts, propose hook + body + image suggestion
- Quote brand voice guide when in doubt; do not invent voice rules

## Decision Rules (how to handle ambiguity)
- For copy length: shorter wins unless I say otherwise
- For tone: warmer wins for top-of-funnel; more direct wins for bottom
- For visual references: see `brand/visual-identity.md` before suggesting

## Gotchas
- Acme's product name changed from "AcmeSync" to "Acme Sync" (space added) in Q3 2025
- Our trademark line is "® Acme Inc." not "© Acme" (check at publish)
- Legal must review any claim about competitors before publishing
- We do NOT mention specific customer names without explicit permission

## See also
- Brand voice guide: `@brand/voice-guide.md`
- Visual identity: `brand/visual-identity.md`
- Past successful campaigns: `campaigns/standouts/`
- Audience personas: `research/personas/`
```

**Total: ~45 lines.**

## Annotations

**"About me" section**: Critical for non-code work. Tells Claude:
- Who I am (role and authority)
- Team members and what they own
- This shapes how Claude interprets requests like "Lisa thinks we should..."

**Layout section**: Maps the workspace. Note `Q1-2026/` and `Q4-2025/` distinction — Claude knows what's current vs historical.

**Target Audience**: Specific demographics, not vague. "B2B SaaS Product Managers, 30-45, technically literate, time-poor" tells Claude exactly who to write for. This shapes vocabulary, examples, and references.

**Voice**: 
- The forbidden words list is critical. These are corporate-speak red flags.
- "Specific numbers over adjectives" is concrete and actionable.
- "Write to one person" is a powerful framing rule.

**Workflow Rules**: The "Lisa thinks X" rule is interesting — it lets Sarah delegate trust. When she says her teammate thinks something, Claude treats it as approved without asking for clarification. This makes the workspace collaborative without ceremony.

The "2-3 options for headlines" rule prevents single-output thinking. Forces variation.

**Decision Rules**: Smart pattern — when ambiguous, here's the default direction. Prevents Claude from getting stuck or asking too many clarifying questions.

**Gotchas**: 
- Product name change is institutional knowledge
- Trademark/copyright distinction is easy to get wrong
- Legal review requirement is a compliance gate
- Customer permission rule prevents accidental name-drops

## What's notably absent

- ❌ "Write good copy" — vague, unhelpful
- ❌ "Use marketing best practices" — meaningless
- ❌ Lengthy brand story (lives in `brand/`)
- ❌ Past campaign details (live in `campaigns/Q4-2025/`)
- ❌ Generic marketing advice

## What this enables

A new session with this CLAUDE.md immediately:
- Knows who Sarah is and her team structure
- Knows which quarter is current
- Writes to the right audience
- Uses correct voice (and avoids corporate-speak)
- Respects approval workflow
- Catches trademark/legal issues
- Doesn't invent customer references

Without it, Claude would:
- Ask basic context questions every session
- Default to generic B2B marketing voice
- Use forbidden words
- Skip approval workflow
- Maybe invent quotes or customer names

## Lessons for other non-code workspaces

1. **"About me" replaces stack section** — for non-code work, who you are matters as much as what tools you use
2. **Forbidden words list is high-leverage** — every domain has its own corporate-speak to avoid
3. **Delegation rules speed up work** — "When I say [teammate] thinks X, trust it" eliminates clarifying questions
4. **Decision rules for ambiguity** — instead of Claude asking 20 questions, give defaults
5. **Compliance gotchas matter** — legal review, trademark, customer permission, etc.

## Variations by role

**PM workspace**: Replace marketing voice with engineering communication style. Add stakeholder map. Add "definition of done" criteria.

**Operations workspace**: Add SLA references. Add escalation paths. Add stakeholder calendar.

**Research workspace**: Add citation requirements. Add evidence quality standards. Add anti-fabrication rules.

Same template structure, different specific rules per role.

## A note on power

This example demonstrates an important point: **CLAUDE.md is NOT just for code**.

Any role where you work with Claude on repeatable tasks can benefit:
- Marketing (this example)
- Project management
- Research and analysis
- Content creation
- HR / People operations
- Customer support training
- Executive support

The principles are universal: identify who you are, what you do, what's non-obvious about your context, and what mistakes to avoid. Then Claude can work as a competent assistant who already knows your context.
