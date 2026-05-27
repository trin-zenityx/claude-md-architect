# Example: Blog / Content Creation (Non-Code)

Shows CLAUDE.md for a creative writing workspace. Voice rules and anti-AI style are the highest-value content.

## The File

```markdown
# Tech Practitioner Blog
Personal blog covering AWS security, AI workflows, and engineering management.
Audience: senior engineers and tech leads.

## Voice
Senior practitioner writing for senior practitioners. Direct, evidence-based, 
no hype. Skeptical of trends. Real war stories preferred over abstract advice.

## Anti-AI Style Rules (apply to every draft)
- No "delve", "leverage", "robust", "seamless", "moreover", "furthermore"
- No em-dashes (use commas or periods)
- No bullet lists of 3 items when prose would work
- Sentences average 15 words; max 25
- Avoid "in today's fast-paced world..." and similar openings
- Open with a concrete scenario, not a definition
- Close with a recommendation or question, not a summary

## Structure (typical post)
- Hook: 1 paragraph, real incident or specific moment
- Problem framing: 2-3 paragraphs explaining what's at stake
- Solution or analysis: with one code or workflow example
- Closing: 1 paragraph, concrete recommendation

## Workspace Layout
- `drafts/` — work in progress
- `published/` — live on the blog
- `references/` — links and quotes I'm considering using
- `voice-calibration/` — past posts whose voice nailed it

## Workflow Rules
- Before drafting a new post, read 2 posts from `voice-calibration/`
- Always propose 3 headline options
- Code examples must be actually-runnable, not pseudocode
- Don't fabricate war stories; if I haven't told you the incident, ask
- Citations link to primary sources, not summaries of summaries

## See also
- Brand voice guide: `voice-calibration/README.md`
- Topics I've covered (for avoiding repeats): `published/index.md`
```

**Total: ~40 lines.**

## Annotations

**Voice section**: The most important section for content work. Three lines, but each one shapes everything. "Senior practitioner writing for senior practitioners" sets register. "Real war stories preferred over abstract advice" sets content priority.

**Anti-AI Style Rules**: This section is gold for creative writing with Claude. The forbidden words ("delve", "leverage", etc.) are the literal tells of AI-generated text. Most experienced writers can spot AI-written content by these markers. Banning them explicitly prevents Claude from defaulting to its training distribution.

Other anti-AI rules:
- "No em-dashes" — extremely common AI tic
- "Sentences average 15 words; max 25" — AI tends to write longer, more uniform sentences
- "Open with a concrete scenario, not a definition" — AI defaults to definitional openings
- "Close with a recommendation or question" — AI defaults to summary closings

**Structure (typical post)**: Gives Claude a template for the form, not just the voice. Specific enough to guide ("Hook: 1 paragraph") but flexible enough not to constrain ("Solution or analysis: with one code or workflow example" — variable length).

**Workflow rules**:
- "Read 2 posts from voice-calibration/" — concrete pre-flight action
- "Propose 3 headline options" — variation request, not single output
- "Code examples must be runnable" — quality gate
- "Don't fabricate war stories" — anti-hallucination measure
- "Citations link to primary sources" — quality standard

## What's notably absent

- ❌ "Be creative" — meaningless instruction
- ❌ "Make it engaging" — too vague to act on
- ❌ "Use markdown" — Claude does by default
- ❌ Spelling/grammar rules — handled at editing stage by other tools
- ❌ Long lists of synonyms — would bloat without much benefit

## What this enables

Claude with this CLAUDE.md will produce drafts that:
- Sound like the author (voice rules)
- Don't smell like AI (anti-AI style rules)
- Follow the author's typical structure
- Use concrete examples (anti-AI rule + workflow rule)
- Don't fabricate (workflow rule)

Without this file, Claude defaults to its training distribution: definitional openings, em-dashes, "delve into", uniform sentences, generic closings. Reads exactly like AI.

## Lessons for other content workspaces

1. **Voice rules first** — without these, everything else is wasted effort
2. **Anti-AI style rules are mandatory** — they're the difference between "writing with Claude" and "AI-generated content"
3. **Structure can be loose** — give a template, not a straightjacket
4. **Point to exemplars** — "read these 2 posts" is more effective than describing voice in prose
5. **Anti-fabrication rules** — for non-fiction especially, explicitly forbid invention

## Variations by content type

**Newsletter**: Add subscriber-relationship language rules ("write to one person, not the list")

**Documentation**: Replace voice rules with clarity rules ("define jargon on first use", "examples before abstractions")

**Marketing**: Add brand voice section + audience pain points reference

**Fiction**: Add character voice consistency, POV rules, world-building references

Same underlying structure, different specific rules per genre.
