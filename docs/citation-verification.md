# Citation Verification Report — claude-md-architect skill

Date: 2026-05-27
Verified by: research subagent using WebSearch + WebFetch

---

## 1. HumanLayer "Writing a good CLAUDE.md"

**Status: VERIFIED (with one nuance)**

- **URL:** https://www.humanlayer.dev/blog/writing-a-good-claude-md
- **Article exists:** Yes, real article, real author (Kyle Mistele).
- **Claim (a) — "<60 lines":** PARTIAL. The article does NOT prescribe <60 lines as a universal rule. It states: *"general consensus is that < 300 lines is best, and shorter is even better"* and mentions HumanLayer's own file *"is less than sixty lines"* as an example, not as a recommendation.
- **Claim (b) — "Claude is not a linter / never send an LLM to do a linter's job":** VERIFIED. Exact phrasing in the article:
  - Section heading: *"Claude is (not) an expensive linter"*
  - Direct quote: *"Never send an LLM to do a linter's job."*
- **Claim (c) — "ignores instructions uniformly, not selectively":** VERIFIED. Exact quote:
  > "As instruction count increases, instruction-following quality decreases uniformly. This means that as you give the LLM more instructions, it doesn't simply ignore the newer ('further down in the file') instructions — it begins to **ignore all of them uniformly**."

**Recommendation:** Keep citation. Fix the "<60 lines" claim — attribute it correctly as "HumanLayer's own CLAUDE.md is under 60 lines" rather than as a prescriptive rule. The actual prescriptive number from the post is "<300 lines, shorter is better."

---

## 2. Anthropic official Claude Code docs (code.claude.com)

**Status: VERIFIED**

- **URL:** https://code.claude.com/docs/en/memory (live, official Anthropic docs)
- **URL:** https://code.claude.com/docs/en/best-practices (live, official)
- **"Under 200 lines" claim:** VERIFIED — direct quote from /memory page:
  > **Size:** target under 200 lines per CLAUDE.md file. Longer files consume more context and reduce adherence.
- Also confirmed: troubleshooting section states *"Files over 200 lines consume more context and may reduce adherence."*
- **/best-practices page:** Does NOT specify a line count, but says: *"Keep it concise. For each line, ask: 'Would removing this cause Claude to make mistakes?' If not, cut it. Bloated CLAUDE.md files cause Claude to ignore your actual instructions!"*
- Bonus finding: Anthropic docs also state auto memory (MEMORY.md) is loaded *"first 200 lines or 25KB, whichever comes first"* — relevant supporting datapoint.

**Recommendation:** Keep citation. Both URLs are canonical and active. The 200-line number is officially Anthropic's recommendation on the /memory page.

---

## 3. Vercel Engineering "AGENTS.md outperforms skills in our agent evals"

**Status: VERIFIED**

- **URL:** https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals
- **Published:** January 27, 2026 (matches "Jan 2026" claim)
- **Real eval results from the article:**
  - Baseline (no docs): 53% pass rate
  - Skills with explicit instructions: 79% pass rate
  - AGENTS.md docs index (8KB compressed): 100% pass rate
- Tested against Next.js 16 APIs across Build, Lint, Test categories.

**Recommendation:** Keep citation. Solid, datable, verifiable — exact numbers can be cited.

---

## 4. Arize AI "CLAUDE.md best practices learned from prompt learning"

**Status: VERIFIED**

- **URL:** https://arize.com/blog/claude-md-best-practices-learned-from-optimizing-claude-code-with-prompt-learning/
- **SWE-Bench gains claim (+5-10%):** VERIFIED. Article reports:
  - **+10% boost on SWE Bench** for Claude Code via prompt-learning optimized system prompt
  - +5% improvement on GitHub issue resolution for Claude (with 150 training examples)
  - +15% for Cline
- Methodology: split SWE Bench Lite into train/test, used about half to train the prompt optimizer, the other half to test.
- GitHub repo also exists: https://github.com/Arize-ai/prompt-learning

**Recommendation:** Keep citation. The "+5-10%" range claim is accurate; could even cite the specific "+10% on SWE Bench" headline number.

---

## 5. Academic paper claim: "150-200 reliable instructions per file"

**Status: PARTIAL — paper exists, exact "150-200" number does NOT**

- **Paper:** arXiv:2507.11538 — "How Many Instructions Can LLMs Follow at Once?"
- **URL:** https://arxiv.org/abs/2507.11538
- **Authors:** Daniel Jaroslawicz, Brendan Whiting, Parth Shah, Karime Maamari (submitted July 15, 2025)
- **Real finding:** Introduces IFScale benchmark with 500 keyword-inclusion instructions. Even frontier models max out at **68% accuracy at 500 instructions**.
- **The specific "150-200 reliable" number is NOT in the paper.** The paper presents a *gradient* of degradation — there is no clean threshold like "150-200 is the reliable ceiling." The paper does observe bias toward earlier instructions and distinct degradation patterns, but doesn't pin a number.

**Recommendation:** Soften this citation. Either:
- (a) Drop the "150-200" specific number and instead say: *"Performance degrades steadily as instruction count grows — frontier models only reach 68% accuracy at 500 instructions (Jaroslawicz et al., arXiv:2507.11538)."*
- (b) Remove the citation entirely if a specific threshold is needed for the skill's argument.

---

## 6. ETH Zurich / Gloaguen et al. "Evaluating AGENTS.md"

**Status: VERIFIED**

- **Paper:** arXiv:2602.11988 — "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?"
- **URL:** https://arxiv.org/abs/2602.11988
- **Authors:** Thibaud Gloaguen, Niels Mündler, Mark Müller, Veselin Raychev, Martin Vechev (ETH Zurich + LogicStar.ai)
- **Published:** February 2026, presented at ICLR 2026 Workshop on Memory for LLM-Based Agentic Systems
- **Real findings:**
  - Built AGENTBENCH: 138 real-world Python software engineering tasks from niche repos
  - Evaluated 4 frontier models in 3 settings: no context file / LLM-generated / human-written
  - **Context files tend to REDUCE task success rates** compared to no repository context
  - Increased inference cost by **over 20%**
- Official ETH SRI Lab page: https://www.sri.inf.ethz.ch/publications/gloaguen2026agentsmd

**Recommendation:** Keep citation. Genuinely useful counterweight to the Vercel finding — interesting tension in the field worth teaching.

---

## 7. GitHub issues for Thai language CLI bug in claude-code

**Status: BOTH VERIFIED**

- **Issue #21149:** https://github.com/anthropics/claude-code/issues/21149
  - Real issue. About Thai output generation in Claude Code v2.1.20.
  - Specifically about sara aa (า) vowel — claude CANNOT generate Thai text containing it.
  - Labeled: `area:a11y`, `area:tui`, `bug`, `has repro`
  - Marked CRITICAL/BLOCKING. Open with no current mitigation except downgrade.
- **Issue #20932:** https://github.com/anthropics/claude-code/issues/20932
  - Real issue. About Thai INPUT in interactive mode.
  - Combining characters (U+0E34-U+0E3A vowels, U+0E47-U+0E4E tone marks) display wrong.
  - Root cause: input library lacks proper `wcwidth()` for Thai combining marks.
  - Status: closed as duplicate of another existing report.

**Recommendation:** Keep both citations. Highly relevant for Trin's Thai user base. Note that #20932 is closed (duplicate), so frame it as "duplicate of an upstream tracked issue" rather than "open."

---

## 8. Boris Cherny

**Status: VERIFIED**

- **Confirmed as creator and Head of Claude Code at Anthropic.**
- **Background:** Previously 5 years at Meta as Principal Engineer; author of "Programming TypeScript" (O'Reilly).
- **LinkedIn:** https://www.linkedin.com/in/bcherny/
- Multiple substantive public interviews and posts:
  - Pragmatic Engineer: https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny
  - Lenny's Newsletter: https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens
  - Developing.dev profile: https://www.developing.dev/p/boris-cherny-creator-of-claude-code
  - Official Anthropic webinar: https://www.anthropic.com/webinars/claude-code-for-financial-services-a-session-with-the-creator-boris-cherny
- Well-known workflow soundbite: *"ships 20-30 PRs a day by running 5 parallel Claude instances."*

**Recommendation:** Keep citation. Solid attribution — he is consistently called "creator of Claude Code" in Anthropic's own materials.

---

## Recommended actions for the skill

### KEEP AS-IS (well-supported)
1. **Anthropic /memory docs — 200-line guidance** (item 2) — official, canonical, current.
2. **Vercel AGENTS.md eval** (item 3) — solid numbers, recent, verifiable.
3. **Arize prompt-learning SWE-Bench results** (item 4) — verified, can even cite specific +10% number.
4. **ETH Zurich AGENTS.md study** (item 6) — verified, useful counterpoint.
5. **GitHub Thai bug issues #21149, #20932** (item 7) — both real, relevant to Trin's audience.
6. **Boris Cherny attribution** (item 8) — solid public figure, multiple official sources.

### SOFTEN / FIX
7. **HumanLayer "<60 lines"** (item 1) — change to: *"HumanLayer's own CLAUDE.md is under 60 lines as an example; their explicit recommendation is <300 lines, shorter is better."* Keep the linter quote and the "ignores uniformly" quote — both verified verbatim.

### REMOVE OR REPHRASE
8. **"150-200 reliable instructions per file" from arXiv:2507.11538** (item 5) — this specific number is NOT in the paper. The paper shows gradual degradation up to 68% at 500 instructions. Either drop the number or rephrase as: *"Instruction-following degrades steadily with density; even frontier models hit only 68% accuracy at 500 instructions (IFScale, arXiv:2507.11538)."*

### OVERALL ASSESSMENT
Of 8 source claims, **7 are verified or mostly verified, and 1 misattributes a number** to a real paper. No source was completely fabricated. The skill is in much better shape than typical AI-generated citation lists — only one specific number needs correcting, plus one nuance fix on HumanLayer's line count. Trin can confidently keep all 8 sources with the small edits noted above.
