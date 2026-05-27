# claude-md-architect

> A Claude Code skill that designs, audits, extracts, and teaches **CLAUDE.md** — the highest-leverage prompt in any Claude Code project.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-d97757)](https://code.claude.com)
[![Language](https://img.shields.io/badge/Language-Thai%20%2B%20English-blue)](#)

---

# 🇹🇭 ภาษาไทย

## ทำไมเรื่องนี้ถึงสำคัญ

ลองนึกภาพคุณรับพนักงานใหม่เข้ามา เก่งมาก IQ 180 ความจำเหมือนช้าง แต่... **ทุกเช้าตื่นมาแล้วลืมหมด**

ทุกครั้งที่คุยกัน คุณต้องอธิบายใหม่:
- เราทำธุรกิจอะไร
- โปรเจกต์ใช้ stack อะไร version ไหน
- โค้ดวางที่ไหน
- เคยพลาดอะไรมาบ้าง อย่าทำซ้ำ

**ฟังดูเหนื่อยใช่ไหม?** นี่คือสิ่งที่เกิดขึ้นทุกครั้งที่คุณเปิด session ใหม่ใน Claude Code

`CLAUDE.md` คือไฟล์ที่ Claude อ่านอัตโนมัติทุกครั้งที่เริ่ม session มันคือ "onboarding doc" ของพนักงานความจำสั้นคนนั้น — เขียนครั้งเดียว ใช้ได้ตลอด

แต่... **เขียนแย่ๆ ก็เจ๊งได้** ผลวิจัยจาก ETH Zurich ([Gloaguen et al., 2026](https://arxiv.org/abs/2602.11988)) พบว่า context file คุณภาพต่ำ **ลด task success rate** เทียบกับไม่มีไฟล์เลย และยังเพิ่มต้นทุนการรัน 20%+ อีกด้วย

skill นี้สอน Claude (และคุณ) ว่า "เขียนยังไงให้ดี"

---

## skill นี้ทำอะไรได้บ้าง

มี **4 modes** Claude เลือกใช้เองตามที่คุณพูด:

| Mode | ใช้เมื่อ | ตัวอย่างคำพูด |
|---|---|---|
| 🆕 **create** | โปรเจกต์ใหม่ ยังไม่มี CLAUDE.md | "ช่วยสร้าง CLAUDE.md ให้โปรเจกต์ Next.js ของผมหน่อย" |
| 📤 **extract** | คุยกับ Claude มาทั้งวัน อยากเก็บ context ก่อนปิด | "ก่อนปิด session นี้ extract context ออกมาเป็น CLAUDE.md" |
| 🔧 **audit** | CLAUDE.md เดิมยาวเกิน Claude เริ่มไม่ตามกฎ | "CLAUDE.md ของผม 400 บรรทัดแล้ว Claude ignore กฎไปครึ่งนึง" |
| 🎓 **teach** | อยากเข้าใจ/สอนคนอื่นเรื่อง CLAUDE.md | "อธิบายให้น้องในทีมฟังหน่อยว่า CLAUDE.md ทำงานยังไง" |

แต่ละ mode มี workflow ละเอียดของตัวเอง พร้อม self-test phase ที่จำลอง session ใหม่อ่านผลลัพธ์ก่อนปิดงาน

---

## ติดตั้ง (3 วิธี)

### วิธีที่ 1: Global (ใช้ทุกโปรเจกต์) — แนะนำ

```bash
cd ~/.claude/skills/
curl -L https://github.com/trin-zenityx/claude-md-architect/releases/latest/download/claude-md-architect.skill \
  -o claude-md-architect.skill
unzip claude-md-architect.skill && rm claude-md-architect.skill
```

หรือ clone repo ตรงๆ:

```bash
git clone https://github.com/trin-zenityx/claude-md-architect.git \
  ~/.claude/skills/claude-md-architect
```

ตรวจสอบ:
```bash
ls ~/.claude/skills/claude-md-architect/SKILL.md
# ถ้าเจอไฟล์ = พร้อมใช้ Claude จะ trigger อัตโนมัติเมื่อพูดถึง CLAUDE.md
```

### วิธีที่ 2: Per-Project (เฉพาะโปรเจกต์)

```bash
cd <project-root>
git clone https://github.com/trin-zenityx/claude-md-architect.git \
  .claude/skills/claude-md-architect
```

### วิธีที่ 3: แจกให้นักเรียน / เพื่อนร่วมทีม

ดาวน์โหลด `claude-md-architect.skill` (zip file 64 KB) จาก [Releases](https://github.com/trin-zenityx/claude-md-architect/releases) — บอกให้ unzip ลง `~/.claude/skills/` จบ

---

## ตัวอย่างการใช้งาน

### ก. สร้าง CLAUDE.md ใหม่จากศูนย์

```
คุณ: ช่วยสร้าง CLAUDE.md ให้โปรเจกต์ Next.js + Supabase + Stripe ของผมหน่อย
      ขายคอร์สภาษาไทย เก็บเงิน THB

Claude: [trigger create mode → ตรวจ package.json + README → ถาม 2-3 คำถามที่
        ตอบจากโค้ดไม่ได้ → เขียน CLAUDE.md 50-70 บรรทัด พร้อม docs/
        ถ้าจำเป็น → ทำ self-test phase 6 simulate session ใหม่]
```

### ข. Audit CLAUDE.md ที่ bloat

```
คุณ: CLAUDE.md ของผมยาวเกินไป Claude เริ่ม ignore กฎ ดูให้หน่อย

Claude: [trigger audit mode → อ่านทุกไฟล์ → diagnostic 10 anti-patterns
        → propose refactor plan ก่อนแก้ → ตัดได้ 60-80% มาตรฐาน
        → ย้ายเนื้อหายาวไป docs/]
```

### ค. Extract context ก่อนปิด session

```
คุณ: เราคุยกันมาทั้งวันเรื่อง LINE bot นี้ ก่อนผมปิด ช่วย extract context
      ออกมาเป็น CLAUDE.md ให้ session พรุ่งนี้รับช่วงต่อได้

Claude: [trigger extract mode → 6 phases:
        Phase 1 dump ทุกอย่าง mark [uncertain] ถ้าไม่ชัด
        Phase 2 ให้คุณ verify ก่อน
        Phase 3 map ว่าอันไหนไปไหน (CLAUDE.md / docs/ / progress.md)
        Phase 4 เขียนไฟล์
        Phase 5 self-test simulate fresh session
        Phase 6 ทำ HANDOFF.md พร้อม starter prompt สำหรับพรุ่งนี้]
```

### ง. สอนคนอื่น

```
คุณ: ผมจะเอาไปสอนนักเรียน 90 นาที ช่วยจัด structure การสอนหน่อย

Claude: [trigger teach mode → calibrate level ของ audience
        → ให้ workshop structure 90 นาที + แบบฝึก + ตัวอย่าง bad→good]
```

---

## หลักการเบื้องหลัง (อยากให้คุณเข้าใจ — ไม่ใช่แค่ใช้ skill เป็น)

### 🎯 หลัก #1: Minimum Viable Context

**ยิ่งกฎเยอะ ≠ Claude ตามกฎมากขึ้น** ตรงข้ามเลย

[HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md) ของ Kyle Mistele สังเกตจาก production sessions จริงว่า:

> *"As instruction count increases, instruction-following quality decreases **uniformly**. The LLM doesn't just ignore newer instructions — it begins to **ignore all of them uniformly**."*

นั่นแปลว่า ถ้าคุณใส่กฎ 200 ข้อ Claude อาจจะตามกฎข้อ #1 ดีกว่าตอนคุณใส่กฎ 500 ข้อ — เพราะกฎ 500 ข้อทำให้มัน "ignore ทุกข้อพร้อมๆ กัน"

งานวิจัยทางวิชาการสนับสนุน: paper **IFScale** ([Jaroslawicz et al., 2025](https://arxiv.org/abs/2507.11538)) ทดสอบ frontier models กับคำสั่ง 500 ข้อ พบว่า accuracy เหลือเพียง **68%** — และตกลงเป็น gradient ไม่ใช่ cliff คุณไม่รู้ตัวว่าผ่าน threshold ไหนแล้ว

📏 **เป้าหมาย:** Anthropic แนะนำอย่างเป็นทางการที่ [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory) ว่า **"target under 200 lines per CLAUDE.md file"** แต่ทีม production จริงๆ ส่วนใหญ่อยู่ที่ **40-80 บรรทัด**

### 🎯 หลัก #2: The Pruning Test (เครื่องมือเดียวที่ต้องจำ)

ทุกบรรทัดใน CLAUDE.md ถามคำถามเดียว:

> **"ถ้าผมลบบรรทัดนี้ออก Claude จะพลาดไหม?"**

- **YES** → เก็บไว้
- **MAYBE** → ตัด (ถ้า Claude พลาดจริง ค่อยใส่กลับ)
- **NO** → ตัดทิ้งแน่นอน

ตัวอย่างจริง:

| บรรทัด | คำตอบ | ทำไง |
|---|---|---|
| "ใช้ 2-space indentation" | NO — Prettier/EditorConfig ทำได้ | ✂️ ตัด |
| "Run pnpm test before commit" | YES — Claude เดาไม่ได้ว่าใช้ pnpm | ✅ เก็บ |
| "You are a senior 10x engineer" | NO — system prompt ของ Claude มี role priming อยู่แล้ว | ✂️ ตัด |
| "Stripe webhook ต้อง validate signature" | YES — เป็น project-specific gotcha | ✅ เก็บ |
| "Customer IDs เป็น string ไม่ใช่ int" | YES — มองจากโค้ดเดียวไม่เห็น | ✅ เก็บ |

### 🎯 หลัก #3: Progressive Disclosure (จัดชั้นความรู้)

CLAUDE.md ควรเป็น **orchestrator** ไม่ใช่ **encyclopedia**

```
project-root/
├── CLAUDE.md              ← 40-80 บรรทัด, โหลดทุก session
├── CLAUDE.local.md        ← .gitignored, ส่วนตัว
├── docs/
│   ├── architecture.md    ← "read when modifying core modules"
│   ├── decisions.md       ← architectural decision log
│   └── gotchas-stripe.md  ← domain-specific issues
└── .claude/
    ├── rules/             ← path-scoped rules
    ├── commands/          ← slash commands
    └── skills/            ← on-demand skills (แบบที่คุณกำลังอ่านนี่)
```

**Key insight:**
- `@docs/file.md` syntax = โหลดตอน startup (ไม่ช่วยประหยัด context)
- Prose pointer แบบ "read docs/X.md when Y" = just-in-time, **ช่วยประหยัดจริง**

ส่วนใหญ่ใช้ prose pointer ดีกว่า — Claude อ่านเมื่อจำเป็นเท่านั้น

### 🎯 หลัก #4: Earn Every Line

กฎทุกข้อต้องสืบย้อนไปถึง "incident จริง" ได้ — ไม่ใช่ "hypothetical หรือว่ากันไว้ดีกว่า"

**Promotion rule:**
- พลาดครั้งเดียว → ปล่อยไว้ใน MEMORY.md (Claude เขียนเอง auto)
- พลาดซ้ำ 2+ ครั้ง → promote ขึ้น CLAUDE.md gotchas section
- พลาดเล็กๆ → แค่บอก Claude inline ตอนนั้น ไม่ต้องใส่ในไฟล์

---

## งานวิจัย & แหล่งอ้างอิง (ทุกอันมีตัวตนจริง — verify มาแล้ว)

skill นี้ไม่ได้เขียนจากการเดา ทุก claim มี evidence ทั้งหมดเป็นไฟล์ที่ verify แล้วใน [`docs/citation-verification.md`](docs/citation-verification.md)

| ที่มา | จะเรียนรู้อะไร |
|---|---|
| [Anthropic Memory Docs](https://code.claude.com/docs/en/memory) | กฎ 200 บรรทัดอย่างเป็นทางการ + pruning test phrasing |
| [HumanLayer — Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md) (Kyle Mistele) | "Claude is not a linter" + "instructions ignored uniformly" |
| [Vercel — AGENTS.md outperforms skills](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals) (Jan 2026) | eval study: baseline 53% → skills 79% → AGENTS.md **100%** |
| [Arize AI — CLAUDE.md best practices from prompt learning](https://arize.com/blog/claude-md-best-practices-learned-from-optimizing-claude-code-with-prompt-learning/) | optimized CLAUDE.md ได้ **+10%** บน SWE-Bench Lite |
| [Gloaguen et al. — Evaluating AGENTS.md](https://arxiv.org/abs/2602.11988) (ETH Zurich, ICLR 2026 workshop) | counterweight: context file คุณภาพต่ำ **ลด** success rate, เพิ่มต้นทุน 20%+ |
| [Jaroslawicz et al. — How Many Instructions Can LLMs Follow at Once?](https://arxiv.org/abs/2507.11538) (IFScale benchmark) | frontier models เหลือ accuracy **68%** ที่ 500 instructions |
| [Boris Cherny](https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens) (Head of Claude Code, Anthropic) | production patterns + วิธีคิดของคนสร้าง Claude Code |

📝 **ข้อสังเกตที่น่าสนใจ:** Vercel บอกว่า AGENTS.md ทำให้ pass rate ดีขึ้นมาก แต่ ETH Zurich บอกว่า context files **ลด** success rate ขัดกัน? ไม่หรอก — **มันขึ้นกับคุณภาพ** ของไฟล์ Vercel ใช้ "8KB compressed docs index" ที่เน้นๆ ETH ใช้ "LLM-generated หรือ human-written" ที่อาจมี boilerplate มาก สรุปคือ:

> *Good context file → ปรับ +47% / Bad context file → ทำลายงาน*

นี่คือเหตุผลที่ skill นี้มี **pruning test** เป็นกลไกหลัก

---

## 10 Anti-Patterns ที่เจอบ่อย (skill ตรวจให้)

| # | Anti-Pattern | สัญญาณ | วิธีแก้ |
|---|---|---|---|
| 1 | **Bloated catch-all** | >200 บรรทัด, kitchen sink | ย้ายไป `docs/` ตามหัวข้อ |
| 2 | **Claude as linter** | "ใช้ 2-space indent", "single quotes" | configure ESLint/Prettier/EditorConfig |
| 3 | **Auto-generated /init untouched** | ดูเหมือน output `claude /init` ที่ไม่ได้แก้ | ตัด 60-80% เก็บแค่ที่ project-specific |
| 4 | **Negation-only constraints** | "Never X" ไม่บอก "do Y instead" | pair กัน — "Never X; instead do Y" |
| 5 | **Personality bloat** | "You are a senior 10x engineer who..." | ลบทิ้งทั้งหมด |
| 6 | **Stale and contradictory** | "ใช้ Redux" + "ใช้ Zustand" coexist | quarterly review |
| 7 | **Conflicting user vs project rules** | `~/.claude/CLAUDE.md` ขัด `./CLAUDE.md` | user level เก็บแค่ universal preference |
| 8 | **One rule per mistake** | gotchas section 300 บรรทัด | one-off → MEMORY.md เท่านั้น, ซ้ำ 2+ ค่อยขึ้น CLAUDE.md |
| 9 | **Import everything via @** | `@docs/a @docs/b @docs/c...` | ใช้ prose pointer แทน |
| 10 | **CLAUDE.md as documentation** | อ่านเหมือน README | README สำหรับมนุษย์, CLAUDE.md สำหรับ Claude |

ดูรายละเอียดพร้อมตัวอย่าง bad → good ที่ [`references/anti-patterns.md`](references/anti-patterns.md)

---

## โครงสร้างไฟล์

```
claude-md-architect/
├── SKILL.md                    ← entry point (Claude อ่านอันนี้ก่อน)
│
├── modes/                      ← 4 mode หลัก โหลด on-demand
│   ├── create.md              ← สร้างใหม่จากศูนย์ (7 phases)
│   ├── extract.md             ← extract จาก session (6 phases, มี variations)
│   ├── audit.md               ← review/refactor (7 phases)
│   └── teach.md               ← สอน (6-layer pedagogical structure)
│
├── references/                 ← knowledge base
│   ├── anti-patterns.md       ← 10 anti-patterns พร้อมตัวอย่าง
│   ├── decision-framework.md  ← เนื้อหาไหนไปอยู่ไหน (decision tree)
│   ├── size-guidelines.md     ← ขนาดที่เหมาะสมต่อไฟล์ + research
│   └── thai-language-notes.md ← สำหรับผู้ใช้ไทย + CLI bugs ที่รู้ตัว
│
├── templates/                  ← starter templates 6 แบบ
│   ├── minimal.md             ← 30 บรรทัด สำหรับมือใหม่
│   ├── standard.md            ← 60 บรรทัด default
│   ├── monorepo.md            ← multi-package
│   ├── non-code-content.md    ← blog/creative writing
│   ├── non-code-research.md   ← data science/research
│   └── self-improving.md      ← มี meta-rules
│
├── examples/                   ← real-world examples พร้อม annotations
│   ├── nextjs-app.md
│   ├── monorepo.md
│   ├── data-science.md
│   ├── blog-content.md
│   └── marketing-workspace.md
│
└── docs/
    └── citation-verification.md  ← audit trail การ verify งานวิจัย
```

---

## สำหรับ instructor / สอนคนอื่น

ถ้าใช้ skill นี้สอน workshop:

1. **เริ่มจาก `teach mode`** — มี layered teaching อัตโนมัติ (calibrate level → 4-6 layers → practice)
2. **ใช้ `examples/`** เป็น case studies — มี annotations อธิบายว่าทำไมแต่ละบรรทัด earn place
3. **แบบฝึกที่ใช้ได้เลย**:
   - ให้นักเรียน audit `references/anti-patterns.md` ตัวอย่าง bad CLAUDE.md
   - ให้เขียน CLAUDE.md สำหรับ project ของตัวเอง 30 บรรทัด
   - สลับกัน peer review โดยใช้ pruning test
4. **Workshop structure 90 นาที** ดูได้ใน [`references/thai-language-notes.md`](references/thai-language-notes.md)

---

## หมายเหตุสำหรับผู้ใช้ภาษาไทย

มี **2 CLI bugs ที่ confirmed** กับ Claude Code v2.1.20:

- 🔴 [Issue #21149](https://github.com/anthropics/claude-code/issues/21149) — *output* bug: Claude generate Thai ที่มี sara aa (า) ไม่ได้ critical, open อยู่
- 🟡 [Issue #20932](https://github.com/anthropics/claude-code/issues/20932) — *input* bug: combining vowels แสดงผิด ตอน type ใน interactive mode (closed as duplicate)

**Workaround:** ใช้ Claude Code Desktop app แทน CLI สำหรับงาน Thai content (file I/O fine ทั้งคู่ — bug แค่ terminal rendering)

แนะนำ pattern: **English structure + Thai content** สำหรับ CLAUDE.md ของโปรเจกต์ไทย ตัวอย่างใน [`references/thai-language-notes.md`](references/thai-language-notes.md)

---

## License

MIT — ใช้ได้อิสระ ทั้งส่วนตัว/สอน/commercial/แจกจ่าย

ถ้าจะ credit: skill นี้ design โดย **Trin Tuchinda / ZenityX** จากการ synthesize งานวิจัยจากชุมชน + งานวิชาการ

---

---

# 🇬🇧 English

## Why this matters

Imagine you hire a new engineer. IQ 180, instant recall — but **wakes up every morning with no memory of yesterday**.

Every conversation starts from zero: what business you're in, what stack the project uses, which versions, where things live, what bugs you've already burned hours on. **Exhausting, right?** This is what happens every time you open a new Claude Code session.

`CLAUDE.md` is the file Claude reads automatically at session start. It's the onboarding doc for that amnesiac engineer — written once, read every time.

But — **bad CLAUDE.md is worse than no CLAUDE.md**. Recent research from ETH Zurich ([Gloaguen et al., 2026](https://arxiv.org/abs/2602.11988)) shows low-quality context files actively **reduce** task success rates compared to no context file at all, while increasing inference cost by 20%+.

This skill teaches Claude (and you) how to write good ones.

---

## What this skill does

Four modes, auto-selected based on what you ask:

| Mode | When | Example trigger |
|---|---|---|
| 🆕 **create** | New project, no CLAUDE.md | "Create a CLAUDE.md for my Next.js project" |
| 📤 **extract** | Long session — preserve context before closing | "Extract our session context into a CLAUDE.md system" |
| 🔧 **audit** | Existing CLAUDE.md is bloated, rules ignored | "My CLAUDE.md is 400 lines and Claude ignores half the rules" |
| 🎓 **teach** | Understand or teach CLAUDE.md to others | "Explain how CLAUDE.md works to my team" |

Each mode has a detailed workflow with a self-test phase that simulates a fresh session reading the output before declaring done.

---

## Install

### Option 1: Global (recommended)

```bash
git clone https://github.com/trin-zenityx/claude-md-architect.git \
  ~/.claude/skills/claude-md-architect
```

Verify: `ls ~/.claude/skills/claude-md-architect/SKILL.md`. Claude Code will auto-trigger the skill when you mention CLAUDE.md.

### Option 2: Per-project

```bash
cd <project-root>
git clone https://github.com/trin-zenityx/claude-md-architect.git \
  .claude/skills/claude-md-architect
```

### Option 3: Distribute to teammates/students

Grab `claude-md-architect.skill` (64 KB zip) from [Releases](https://github.com/trin-zenityx/claude-md-architect/releases). Tell them to unzip into `~/.claude/skills/`. Done.

---

## Core principles you should understand

### 🎯 Principle #1: Minimum Viable Context

**More rules ≠ more rules followed.** Counter-intuitive but well-evidenced.

[HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md) observed from production sessions:

> *"As instruction count increases, instruction-following quality decreases **uniformly**. The LLM doesn't just ignore newer instructions — it begins to **ignore all of them uniformly**."*

Academic backup: the **IFScale** benchmark ([Jaroslawicz et al., 2025](https://arxiv.org/abs/2507.11538)) tested frontier models with 500 instructions — accuracy dropped to **68%**, and the decline is a gradient, not a cliff. You don't know when you've crossed the threshold.

📏 **Target:** Anthropic's [official memory docs](https://code.claude.com/docs/en/memory) say *"target under 200 lines per CLAUDE.md file"*. Production teams typically operate at **40-80 lines**.

### 🎯 Principle #2: The Pruning Test (the one tool to remember)

For every line in CLAUDE.md, ask:

> **"If I removed this line, would Claude make a mistake?"**

- **YES** → keep it
- **MAYBE** → cut it (add back if you observe the mistake)
- **NO** → definitely cut

Real examples:

| Line | Verdict | Action |
|---|---|---|
| "Use 2-space indentation" | NO — Prettier/EditorConfig handles it | ✂️ Cut |
| "Run pnpm test before commit" | YES — Claude can't guess your scripts | ✅ Keep |
| "You are a senior 10x engineer" | NO — Claude Code's system prompt already primes role | ✂️ Cut |
| "Stripe webhook must validate signature" | YES — project-specific gotcha | ✅ Keep |
| "Customer IDs are strings, not integers" | YES — not visible from a single file | ✅ Keep |

### 🎯 Principle #3: Progressive Disclosure

CLAUDE.md should be an **orchestrator**, not an **encyclopedia**.

```
project-root/
├── CLAUDE.md              ← 40-80 lines, loads every session
├── CLAUDE.local.md        ← .gitignored, personal
├── docs/
│   ├── architecture.md    ← "read when modifying core modules"
│   ├── decisions.md       ← architectural decision log
│   └── gotchas-stripe.md  ← domain-specific
└── .claude/
    ├── rules/             ← path-scoped
    ├── commands/          ← slash commands
    └── skills/            ← on-demand (this skill lives here)
```

**Key insight:**
- `@docs/file.md` = loads at startup (no context savings)
- Prose pointer "read docs/X.md when Y" = just-in-time, **actual savings**

For most cross-cutting concerns, prose pointer is correct.

### 🎯 Principle #4: Earn Every Line

Every rule should trace back to a real incident — not a hypothetical one.

**Promotion rule:**
- One-off mistake → leave in MEMORY.md (Claude writes it auto)
- Same mistake 2+ times → promote to CLAUDE.md gotchas
- Trivial mistake → just correct Claude inline, don't file it

---

## Research & sources (all verified real — see [`docs/citation-verification.md`](docs/citation-verification.md))

| Source | What you'll learn |
|---|---|
| [Anthropic Memory Docs](https://code.claude.com/docs/en/memory) | Official 200-line target + canonical pruning test phrasing |
| [HumanLayer — Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md) by Kyle Mistele | "Claude is not a linter" + "instructions ignored uniformly" |
| [Vercel — AGENTS.md outperforms skills](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals) (Jan 2026) | Eval: 53% baseline → 79% skills → **100%** AGENTS.md |
| [Arize AI — CLAUDE.md best practices from prompt learning](https://arize.com/blog/claude-md-best-practices-learned-from-optimizing-claude-code-with-prompt-learning/) | Optimized CLAUDE.md = **+10%** on SWE-Bench Lite |
| [Gloaguen et al. — Evaluating AGENTS.md](https://arxiv.org/abs/2602.11988) (ETH Zurich, ICLR 2026 workshop) | Counterweight: low-quality context files **reduce** success, +20% cost |
| [Jaroslawicz et al. — How Many Instructions Can LLMs Follow at Once?](https://arxiv.org/abs/2507.11538) | Frontier models hit only **68%** accuracy at 500 instructions |
| [Boris Cherny](https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens) (Head of Claude Code) | Production patterns from the creator of Claude Code |

📝 **Interesting tension:** Vercel says AGENTS.md improves pass rate; ETH Zurich says context files **reduce** success. Both true — **quality matters**. Vercel used an 8KB curated docs index. ETH used "LLM-generated or human-written" files that often contained boilerplate. The takeaway:

> *Good context file → +47%. Bad context file → degrades performance.*

This is why this skill makes the **pruning test** its central mechanism.

---

## 10 Anti-Patterns the skill detects

| # | Pattern | Signal | Fix |
|---|---|---|---|
| 1 | **Bloated catch-all** | >200 lines, kitchen sink | Split into `docs/` by topic |
| 2 | **Claude as linter** | "Use 2-space indent", "single quotes" | Configure ESLint/Prettier |
| 3 | **Auto-generated `/init` untouched** | Looks like raw `claude /init` output | Cut 60-80%; keep only project-specific |
| 4 | **Negation-only constraints** | "Never X" without "do Y instead" | Pair them |
| 5 | **Personality bloat** | "You are a senior 10x engineer..." | Delete all of it |
| 6 | **Stale + contradictory** | "Use Redux" + "Use Zustand" coexist | Quarterly review |
| 7 | **Conflicting user vs project** | `~/.claude/CLAUDE.md` vs `./CLAUDE.md` disagree | User-level: only truly universal |
| 8 | **One rule per mistake** | 300-line gotchas section | One-offs → MEMORY.md only |
| 9 | **Import everything via @** | `@docs/a @docs/b @docs/c...` | Use prose pointers |
| 10 | **CLAUDE.md as docs** | Reads like a README | README is for humans; CLAUDE.md is for Claude |

Full bad → good examples: [`references/anti-patterns.md`](references/anti-patterns.md)

---

## For instructors / teaching

If using this skill for workshops:

1. **Start with `teach mode`** — auto-layered teaching (calibrate level → 4-6 layers → practice)
2. **Use `examples/`** as case studies — each has annotations explaining why every line earns its place
3. **Ready-to-use exercises**:
   - Have students audit the bad CLAUDE.md in `references/anti-patterns.md`
   - Write a 30-line CLAUDE.md for their own project
   - Pair-review using the pruning test
4. **90-minute workshop structure** in [`references/thai-language-notes.md`](references/thai-language-notes.md)

---

## Notes for Thai users

Two confirmed CLI bugs in Claude Code v2.1.20:

- 🔴 [Issue #21149](https://github.com/anthropics/claude-code/issues/21149) — *output* bug: Claude can't generate Thai containing sara aa (า). Critical, still open.
- 🟡 [Issue #20932](https://github.com/anthropics/claude-code/issues/20932) — *input* bug: Thai combining vowels render incorrectly when typing in interactive mode (closed as duplicate).

**Workaround:** Use Claude Code Desktop app instead of CLI for Thai content. File I/O works fine in both — bug is terminal rendering only.

Recommended pattern: **English structure + Thai content** for Thai-project CLAUDE.md. Examples in [`references/thai-language-notes.md`](references/thai-language-notes.md).

---

## License

MIT — free for any use: personal, teaching, commercial, redistribution.

Credit: designed by **Trin Tuchinda / ZenityX** as a synthesis of community practitioner writing and academic research.

---

## Contributing

Found a bug or have a suggestion? [Open an issue](https://github.com/trin-zenityx/claude-md-architect/issues).

Built something cool with this skill? PRs welcome — especially:
- More examples in `examples/`
- More templates in `templates/` (e.g., specific frameworks)
- Translations of the skill body to other languages

---

*Built with 🧡 in Bangkok by [ZenityX](https://zenityx.com). If this skill helps you, share it with a teammate.*
