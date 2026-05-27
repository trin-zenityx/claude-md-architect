<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/brand/zenityx-primary-on-dark.png">
    <img alt="ZenityX" src="assets/brand/zenityx-primary.png" width="360">
  </picture>
</p>

<h1 align="center">claude-md-architect</h1>

<p align="center">
  <em>A Claude Code skill from <a href="https://zenityxai.com">ZenityX</a> — Thailand's hands-on AI institute</em><br/>
  <strong>จับมือทำจริง ใช้ได้จริง · Hands-on AI. Real results.</strong>
</p>

<p align="center">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"></a>
  <a href="https://code.claude.com"><img src="https://img.shields.io/badge/Claude%20Code-Skill-d97757" alt="Claude Code Skill"></a>
  <img src="https://img.shields.io/badge/Docs-Thai%20%2B%20English-blue" alt="Docs: Thai + English">
  <a href="https://zenityxai.com"><img src="https://img.shields.io/badge/Made%20by-ZenityX-FF0000?labelColor=000000" alt="Made by ZenityX"></a>
</p>

---

> A Claude Code skill that designs, audits, and extracts **CLAUDE.md** — the highest-leverage prompt in any Claude Code project. Plus a full curriculum on the principles in this README.

**This README is the curriculum.** The skill is the doer. Read this if you want to *understand* CLAUDE.md; install the skill if you want Claude to *do* CLAUDE.md work for you. Both, ideally.

---

# ภาษาไทย

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

skill นี้สอน Claude ให้ "เขียนดี" — และ README นี้สอน *คุณ* ให้เข้าใจว่าทำไมถึงดี

---

## skill นี้ทำอะไรได้บ้าง

มี **3 modes** Claude เลือกใช้เองตามที่คุณพูด:

| Mode | ใช้เมื่อ | ตัวอย่างคำพูด |
|---|---|---|
| **create** | โปรเจกต์ใหม่ ยังไม่มี CLAUDE.md | "ช่วยสร้าง CLAUDE.md ให้โปรเจกต์ Next.js ของผมหน่อย" |
| **extract** | คุยกับ Claude มาทั้งวัน อยากเก็บ context ก่อนปิด | "ก่อนปิด session นี้ extract context ออกมาเป็น CLAUDE.md" |
| **audit** | CLAUDE.md เดิมยาวเกิน Claude เริ่มไม่ตามกฎ | "CLAUDE.md ของผม 400 บรรทัดแล้ว Claude ignore กฎไปครึ่งนึง" |

แต่ละ mode มี workflow ละเอียดของตัวเอง (6-7 phases) พร้อม self-test phase ที่จำลอง session ใหม่อ่านผลลัพธ์ก่อนปิดงาน

> **คำถามแนวคิด** ("CLAUDE.md ทำงานยังไง?", "ต่างจาก CLAUDE.local.md ยังไง?") — ตอบจาก README นี้ได้เลย ไม่ต้องเรียก skill skill ออกแบบมาให้ *ทำงาน* ไม่ใช่ *สอน* (เพื่อให้ context ของ Claude ในงานจริงไม่ถูก dilute ด้วยเนื้อหาสอน)

---

## ติดตั้ง (3 วิธี)

### วิธีที่ 1: Global (ใช้ทุกโปรเจกต์) — แนะนำ

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

ดาวน์โหลด `claude-md-architect.skill` (zip file ~64 KB) จาก [Releases](https://github.com/trin-zenityx/claude-md-architect/releases) — บอกให้ unzip ลง `~/.claude/skills/` จบ

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

---

# หลักการเบื้องหลัง — เรียนรู้ก่อนเริ่มเขียน

ส่วนนี้คือ "คอร์ส" จริงๆ ถ้าคุณอ่านจบเข้าใจ คุณจะเขียน CLAUDE.md ดีกว่าคนส่วนใหญ่ทันที

## หลักการ #1: Minimum Viable Context

**ยิ่งกฎเยอะ ≠ Claude ตามกฎมากขึ้น** ตรงข้ามเลย

[HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md) ของ Kyle Mistele สังเกตจาก production sessions จริงว่า:

> *"As instruction count increases, instruction-following quality decreases **uniformly**. The LLM doesn't just ignore newer instructions — it begins to **ignore all of them uniformly**."*

นั่นแปลว่า ถ้าคุณใส่กฎ 200 ข้อ Claude อาจจะตามกฎข้อ #1 ดีกว่าตอนคุณใส่กฎ 500 ข้อ — เพราะกฎ 500 ข้อทำให้มัน "ignore ทุกข้อพร้อมๆ กัน"

งานวิจัยทางวิชาการสนับสนุน: paper **IFScale** ([Jaroslawicz et al., 2025](https://arxiv.org/abs/2507.11538)) ทดสอบ frontier models กับคำสั่ง 500 ข้อ พบว่า accuracy เหลือเพียง **68%** — และตกลงเป็น gradient ไม่ใช่ cliff คุณไม่รู้ตัวว่าผ่าน threshold ไหนแล้ว

**เป้าหมาย:** Anthropic แนะนำอย่างเป็นทางการที่ [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory) ว่า **"target under 200 lines per CLAUDE.md file"** แต่ทีม production จริงๆ ส่วนใหญ่อยู่ที่ **40-80 บรรทัด**

## หลักการ #2: The Pruning Test (เครื่องมือเดียวที่ต้องจำ)

ทุกบรรทัดใน CLAUDE.md ถามคำถามเดียว:

> **"ถ้าผมลบบรรทัดนี้ออก Claude จะพลาดไหม?"**

- **YES** → เก็บไว้
- **MAYBE** → ตัด (ถ้า Claude พลาดจริง ค่อยใส่กลับ)
- **NO** → ตัดทิ้งแน่นอน

### ฝึกกับตัวเอง

ลองตัดสินใจในใจก่อน แล้วดูเฉลย:

| บรรทัด | คุณคิดว่า? |
|---|---|
| 1. "ใช้ 2-space indent" | ... |
| 2. "Run `pnpm test` ก่อน commit" | ... |
| 3. "You are a senior 10x engineer" | ... |
| 4. "Stripe webhook ต้อง validate signature" | ... |
| 5. "ใช้ meaningful variable names" | ... |

<details>
<summary><b>👉 เฉลย (กดดู)</b></summary>

| บรรทัด | คำตอบ | ทำไม |
|---|---|---|
| 1. "ใช้ 2-space indent" | ❌ **ตัด** | Prettier/EditorConfig ทำให้ Claude ไม่ต้องรู้ |
| 2. "Run pnpm test ก่อน commit" | ✅ **เก็บ** | Claude เดาไม่ได้ว่าใช้ pnpm หรือ npm หรือ yarn |
| 3. "You are a senior 10x engineer" | ❌ **ตัด** | Claude มี role priming ในระบบอยู่แล้ว เปลือง token |
| 4. "Stripe webhook ต้อง validate signature" | ✅ **เก็บ** | Project-specific gotcha — ดูจากโค้ดเดียวไม่เห็น |
| 5. "ใช้ meaningful variable names" | ❌ **ตัด** | Generic advice — Claude รู้อยู่แล้ว |

</details>

ทำได้กี่ข้อจาก 5? ถ้าได้ 3+ = เข้าใจหลัก

## หลักการ #3: Progressive Disclosure (จัดชั้นความรู้)

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

**Key insight ที่คนพลาดเยอะ:**
- `@docs/file.md` syntax = โหลดตอน startup (**ไม่ช่วยประหยัด** context — แค่จัดระเบียบ)
- Prose pointer แบบ "read docs/X.md when Y" = just-in-time, **ช่วยประหยัดจริง**

ส่วนใหญ่ใช้ prose pointer ดีกว่า — Claude อ่านเมื่อจำเป็นเท่านั้น

## หลักการ #4: Earn Every Line

กฎทุกข้อต้องสืบย้อนไปถึง "incident จริง" ได้ — ไม่ใช่ "hypothetical หรือว่ากันไว้ดีกว่า"

**Promotion rule:**
- พลาดครั้งเดียว → ปล่อยไว้ใน MEMORY.md (Claude เขียนเอง auto)
- พลาดซ้ำ 2+ ครั้ง → promote ขึ้น CLAUDE.md gotchas section
- พลาดเล็กๆ → แค่บอก Claude inline ตอนนั้น ไม่ต้องใส่ในไฟล์

---

## Anatomy — รูปร่างของ CLAUDE.md ที่ดี

```markdown
# Course Platform                          ← ชื่อ + 1 บรรทัดอธิบาย
Next.js 15 + Supabase + Stripe (THB)        เก็บ version เสมอ

## Commands                                ← bash ที่ Claude เดาไม่ได้
- `pnpm dev` — start dev server (port 3000)
- `pnpm test -- src/api/foo.test.ts` — run single test
- `pnpm typecheck && pnpm lint` — required before commit

## Structure                               ← folder ที่ไม่ obvious
- `app/` — Next.js App Router pages
- `lib/stripe/` — wrappers — ใช้ตัวนี้ ไม่ใช่ stripe SDK ตรงๆ

## Rules                                   ← convention (paired prohibition + direction)
- ใช้ named exports ไม่ใช้ default
- Server components by default, "use client" เฉพาะเมื่อจำเป็น
- Never use --legacy-peer-deps; upgrade conflict packages instead

## Gotchas                                 ← บั๊กจริงที่เคยเสียเวลาแก้
- Stripe webhook ต้อง `runtime = 'nodejs'` (edge runtime ไม่ work)
- Customer ID เก็บเป็น string ไม่ใช่ int

## See also                                ← pointer ไป docs ที่ไม่ต้องโหลดทุก session
- Architecture: read `docs/architecture.md` when modifying checkout
- Stripe details: `docs/stripe-guide.md`
```

ทั้งไฟล์ **~50 บรรทัด** — ทุกบรรทัด earn its place

ตัวอย่างเพิ่มเติม 5 แบบดูได้ใน [`examples/`](examples/)

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

**ข้อสังเกตที่น่าสนใจ:** Vercel บอกว่า AGENTS.md ทำให้ pass rate ดีขึ้นมาก แต่ ETH Zurich บอกว่า context files **ลด** success rate ขัดกัน? ไม่หรอก — **มันขึ้นกับคุณภาพ** ของไฟล์ Vercel ใช้ "8KB compressed docs index" ที่เน้นๆ ETH ใช้ "LLM-generated หรือ human-written" ที่อาจมี boilerplate มาก สรุปคือ:

> *Good context file → ปรับ +47% / Bad context file → ทำลายงาน*

นี่คือเหตุผลที่ skill นี้มี **pruning test** เป็นกลไกหลัก

---

## สำหรับ instructor / สอนคนอื่น

README นี้ออกแบบให้เป็น *คอร์ส* ในตัวเอง — แต่ถ้าสอน workshop ใช้ structure ด้านล่างนี้ได้เลย

### Suggested Workshop Structure (90 นาที)

```
0:00–0:10   Introduction
            - CLAUDE.md คืออะไร (อ่านส่วน "ทำไมเรื่องนี้ถึงสำคัญ")
            - ทำไมต้องเรียน (relevance)

0:10–0:25   Layer 1: How it works
            - File location + auto-loading mechanism
            - Demo: เปิด session ใหม่ → Claude อ่าน CLAUDE.md อัตโนมัติ

0:25–0:40   Layer 2: Why size matters (attention budget)
            - Token cost vs attention degradation
            - The pruning test (สำคัญที่สุด — เล่นเกมเฉลย 5 บรรทัดด้านบนพร้อมกัน)

0:40–0:55   Layer 3: Anatomy
            - Walk through "Anatomy — รูปร่างของ CLAUDE.md ที่ดี" section
            - แบบฝึก: เขียน CLAUDE.md 30 บรรทัดสำหรับ project ของตัวเอง

0:55–1:10   Layer 4: Anti-patterns
            - Top 5 anti-patterns พร้อมตัวอย่าง bad → good
            - Audit ตัวอย่าง bloated CLAUDE.md ด้วยกัน (ดูใน references/anti-patterns.md)

1:10–1:25   Hands-on
            - นักเรียนใช้ skill จริงๆ — รัน create mode บน project ของตัวเอง
            - Pair review with pruning test

1:25–1:30   Closing
            - Quarterly review เป็น cadence ที่ healthy
            - Resources + next steps
```

### Pronunciation guide (สำหรับสอนเด็กไทย)

First mention ของศัพท์ — ให้ pronunciation ภาษาไทย:

- **CLAUDE.md** = "คลอด เอ็ม ดี" หรือ "คลอด ด๊อท เอ็ม ดี"
- **Token** = "โทเค็น"
- **Context** = "คอนเท็กซ์"
- **Skill** = "สกิล"
- **Prompt** = "พรอมพ์"
- **Pruning test** = ไม่ต้องแปล แต่อธิบายว่า "การตัดของไม่จำเป็นออก"

### ศัพท์ที่ควรเก็บเป็นอังกฤษ (อย่าแปล)

นักเรียนจะเจอใน docs ภาษาอังกฤษ การแปลทำให้สับสน:
- CLAUDE.md, SKILL.md (ชื่อไฟล์)
- Token, context, attention
- Pruning test, Progressive disclosure, Anti-patterns
- Bash, terminal, CLI
- Worktree, branch, commit
- @import, slash commands

### Common Confusions ที่ต้องเตรียมตอบ

| คำถาม | คำตอบสั้นๆ |
|---|---|
| "CLAUDE.md กับ CLAUDE.local.md ต่างกันยังไง" | CLAUDE.md = shared (commit), CLAUDE.local.md = personal (.gitignored) |
| "MEMORY.md คืออะไร" | Claude เขียนเอง auto ต่างจาก CLAUDE.md ที่คนเขียน |
| "ทำไมไม่ใช่ system prompt" | จริงๆ ถูกส่งเป็น user message แต่ทำงานคล้าย system prompt |
| "Token คือตัวอักษรใช่ไหม" | คล้ายๆ ตัวอักษร แต่อังกฤษ 1 token ≈ 4 chars ไทย 1 token ≈ 1–2 chars |
| "ทำไมไม่ใส่ทุกอย่างใน CLAUDE.md" | ยิ่งใส่เยอะ Claude ยิ่งไม่ตาม — show IFScale data |

### แบบฝึกพร้อมใช้ 4 ข้อ

1. **Spot the anti-pattern** — แสดง CLAUDE.md ที่ bloat ให้ดู ([`references/anti-patterns.md`](references/anti-patterns.md) มีตัวอย่าง) ให้นักเรียนชี้ว่าเจอ anti-pattern ที่เท่าไรบ้าง
2. **Apply the pruning test** — แสดง 10 บรรทัด ให้นักเรียนเลือกอันที่ควรเก็บ
3. **Refactor this** — ให้ไฟล์ 200 บรรทัด ให้นักเรียน refactor เหลือ 50
4. **Write from scratch** — ให้ project spec ให้นักเรียนเขียน CLAUDE.md เอง

---

## โครงสร้างไฟล์

```
claude-md-architect/
├── SKILL.md                    ← entry point (Claude อ่านอันนี้ก่อน)
│
├── modes/                      ← 3 mode หลัก โหลด on-demand
│   ├── create.md              ← สร้างใหม่จากศูนย์ (7 phases)
│   ├── extract.md             ← extract จาก session (6 phases, มี variations)
│   └── audit.md               ← review/refactor (7 phases)
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

## License

MIT — ใช้ได้อิสระ ทั้งส่วนตัว/สอน/commercial/แจกจ่าย

ถ้าจะ credit: skill นี้ design โดย **Trin Tuchinda / ZenityX** จากการ synthesize งานวิจัยจากชุมชน + งานวิชาการ

---

---

# English

## Why this matters

Imagine you hire a new engineer. IQ 180, instant recall — but **wakes up every morning with no memory of yesterday**.

Every conversation starts from zero: what business you're in, what stack the project uses, which versions, where things live, what bugs you've already burned hours on. **Exhausting, right?** This is what happens every time you open a new Claude Code session.

`CLAUDE.md` is the file Claude reads automatically at session start. It's the onboarding doc for that amnesiac engineer — written once, read every time.

But — **bad CLAUDE.md is worse than no CLAUDE.md**. Recent research from ETH Zurich ([Gloaguen et al., 2026](https://arxiv.org/abs/2602.11988)) shows low-quality context files actively **reduce** task success rates compared to no context file at all, while increasing inference cost by 20%+.

This skill teaches Claude to write good ones — and this README teaches *you* the principles.

---

## What this skill does

Three modes, auto-selected based on what you ask:

| Mode | When | Example trigger |
|---|---|---|
| **create** | New project, no CLAUDE.md | "Create a CLAUDE.md for my Next.js project" |
| **extract** | Long session — preserve context before closing | "Extract our session context into a CLAUDE.md system" |
| **audit** | Existing CLAUDE.md is bloated, rules ignored | "My CLAUDE.md is 400 lines and Claude ignores half the rules" |

Each mode has a detailed phased workflow with a self-test phase that simulates a fresh session reading the output before declaring done.

> **Conceptual questions** ("Explain how CLAUDE.md works", "What's the difference between CLAUDE.md and CLAUDE.local.md?") are answered by this README. The skill is built to *do work*, not to *teach* (so Claude's task context doesn't get diluted by pedagogy mid-job).

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

Grab `claude-md-architect.skill` (~64 KB zip) from [Releases](https://github.com/trin-zenityx/claude-md-architect/releases). Tell them to unzip into `~/.claude/skills/`. Done.

---

## Core principles — read these before writing your first CLAUDE.md

### Principle #1: Minimum Viable Context

**More rules ≠ more rules followed.** Counter-intuitive but well-evidenced.

[HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md) observed from production sessions:

> *"As instruction count increases, instruction-following quality decreases **uniformly**. The LLM doesn't just ignore newer instructions — it begins to **ignore all of them uniformly**."*

Academic backup: the **IFScale** benchmark ([Jaroslawicz et al., 2025](https://arxiv.org/abs/2507.11538)) tested frontier models with 500 instructions — accuracy dropped to **68%**, and the decline is a gradient, not a cliff. You don't know when you've crossed the threshold.

**Target:** Anthropic's [official memory docs](https://code.claude.com/docs/en/memory) say *"target under 200 lines per CLAUDE.md file"*. Production teams typically operate at **40-80 lines**.

### Principle #2: The Pruning Test (the one tool to remember)

For every line in CLAUDE.md, ask:

> **"If I removed this line, would Claude make a mistake?"**

- **YES** → keep it
- **MAYBE** → cut it (add back if you observe the mistake)
- **NO** → definitely cut

Real examples:

| Line | Verdict | Reason |
|---|---|---|
| "Use 2-space indentation" | ❌ Cut | Prettier/EditorConfig handles it |
| "Run pnpm test before commit" | ✅ Keep | Claude can't guess your scripts |
| "You are a senior 10x engineer" | ❌ Cut | Claude Code's system prompt already primes role |
| "Stripe webhook must validate signature" | ✅ Keep | Project-specific gotcha |
| "Use meaningful variable names" | ❌ Cut | Generic advice Claude knows |

### Principle #3: Progressive Disclosure

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

**Key insight people miss:**
- `@docs/file.md` = loads at startup (**no context savings** — just organization)
- Prose pointer "read docs/X.md when Y" = just-in-time, **actual savings**

For most cross-cutting concerns, prose pointer is correct.

### Principle #4: Earn Every Line

Every rule should trace back to a real incident — not a hypothetical one.

**Promotion rule:**
- One-off mistake → leave in MEMORY.md (Claude writes it auto)
- Same mistake 2+ times → promote to CLAUDE.md gotchas
- Trivial mistake → just correct Claude inline, don't file it

---

## Anatomy — what a good CLAUDE.md looks like

```markdown
# Course Platform                          ← name + 1-line description
Next.js 15 + Supabase + Stripe (THB)        always include versions

## Commands                                ← bash Claude can't guess
- `pnpm dev` — start dev server (port 3000)
- `pnpm test -- src/api/foo.test.ts` — run single test
- `pnpm typecheck && pnpm lint` — required before commit

## Structure                               ← non-obvious folders
- `app/` — Next.js App Router pages
- `lib/stripe/` — wrappers — use these, not stripe SDK directly

## Rules                                   ← paired prohibition + direction
- Use named exports, not default exports
- Server components by default, "use client" only when needed
- Never use --legacy-peer-deps; upgrade conflict packages instead

## Gotchas                                 ← real bugs that wasted time
- Stripe webhook must `runtime = 'nodejs'` (edge runtime breaks)
- Customer ID is a string, not int

## See also                                ← pointers to on-demand docs
- Architecture: read `docs/architecture.md` when modifying checkout
- Stripe details: `docs/stripe-guide.md`
```

Total: **~50 lines** — every line earns its place.

More examples in [`examples/`](examples/).

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

**Interesting tension:** Vercel says AGENTS.md improves pass rate; ETH Zurich says context files **reduce** success. Both true — **quality matters**. Vercel used an 8KB curated docs index. ETH used "LLM-generated or human-written" files that often contained boilerplate. The takeaway:

> *Good context file → +47%. Bad context file → degrades performance.*

This is why this skill makes the **pruning test** its central mechanism.

---

## For instructors

This README is designed to be a self-contained course. If you're running a workshop, the structure below works:

### 90-minute workshop

```
0:00–0:10   Introduction
            - Why CLAUDE.md exists (read "Why this matters")
            - Where it fits in Claude Code's mental model

0:10–0:25   Layer 1: How it works
            - File location, auto-loading
            - Demo: open a new session, watch Claude consume CLAUDE.md

0:25–0:40   Layer 2: Why size matters
            - Token cost vs. attention degradation
            - Play the pruning test quiz (5-line table above) together

0:40–0:55   Layer 3: Anatomy
            - Walk through the "Anatomy" section
            - Exercise: write a 30-line CLAUDE.md for the student's own project

0:55–1:10   Layer 4: Anti-patterns
            - Top 5 with bad → good examples
            - Audit a bloated CLAUDE.md together (use references/anti-patterns.md)

1:10–1:25   Hands-on
            - Students use the skill on their own project (create mode)
            - Pair-review using the pruning test

1:25–1:30   Closing
            - Quarterly review as a healthy cadence
            - Resources + next steps
```

### Ready-to-use exercises

1. **Spot the anti-pattern** — show a bloated CLAUDE.md; have students identify which of the 10 anti-patterns are present
2. **Apply the pruning test** — show 10 lines; have students vote on which to keep
3. **Refactor this** — give a 200-line file; have students cut to 50
4. **Write from scratch** — give a project spec; have students write a CLAUDE.md

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

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/brand/zenityx-primary-on-dark.png">
    <img alt="ZenityX" src="assets/brand/zenityx-primary.png" width="240">
  </picture>
</p>

<h2 align="center">About ZenityX</h2>

<p align="center">
  <strong>ศูนย์การเรียนรู้ด้าน AI ของไทย</strong> · Thailand's hands-on AI institute<br/>
  <em>จับมือทำจริง ใช้ได้จริง · Hands-on AI. Real results.</em>
</p>

---

### Why we built this skill

ZenityX สอน AI แบบจับมือทำทุกขั้นตอน — เราเห็นนักเรียนเสียเวลาเขียน CLAUDE.md ที่ Claude ไม่ตามกฎ เพราะ pattern ที่ทำให้ context file ดี vs. แย่ ไม่ได้ถูก document ไว้ที่เดียว เราเลย synthesize งานวิจัยจาก Anthropic, HumanLayer, Vercel, Arize, และ ETH Zurich แล้ว package เป็น skill นี้ให้ใช้ฟรีและสอนต่อได้ทันที

> *ZenityX teaches AI hands-on, step by step. We watched students burn hours on CLAUDE.md files that Claude ignored — because the patterns that separate good context files from bad ones aren't documented in one place. So we synthesized the research from Anthropic, HumanLayer, Vercel, Arize, and ETH Zurich into a skill anyone can install, use free, and teach from immediately.*

### Connect with us

| | |
|---|---|
| **Website** | [zenityxai.com](https://zenityxai.com) |
| **Facebook** | [@zenityXAiStudio](https://www.facebook.com/zenityXAiStudio) |
| **Instagram** | [@zenityx.th](https://instagram.com/zenityx.th) |
| **YouTube** | [@ZenityXAIStudio](https://youtube.com/@ZenityXAIStudio) |
| **TikTok** | [@zenityxai](https://tiktok.com/@zenityxai) |
| **LINE OA** | `@zenityx` |
| **Email** | admin@zenityxai.com |
| **Phone** | 062-262-9644 |
| **Hours** | เปิดทุกวัน 9:00–18:00 น. (open daily) |

### Want hands-on AI training in Thai?

Online via AnyDesk · Onsite at our Bangkok institute · In-house for organizations

ZenityX สอนทั้ง **Creative AI** (ภาพ/วิดีโอ/เสียง) และ **Business AI** (ทำงานเร็วขึ้น/ลูกค้าเข้า) — เรียนจบแล้วต้องใช้ได้จริง ไม่ใช่แค่รู้ว่ามี

→ [zenityxai.com](https://zenityxai.com)

---

<p align="center">
  <sub>
    <strong>บริษัท เซนนิตี้เอ็กซ์ จำกัด</strong> · ZenityX Co., Ltd. · Tax ID 0205568044595<br/>
    171/2 ซอยลาดพร้าว 53 (โชคชัย 4) แขวงสะพานสอง เขตวังทองหลาง กรุงเทพมหานคร 10310<br/>
    <em>171/2 Soi Lat Phrao 53, Saphan Song, Wang Thonglang, Bangkok 10310, Thailand</em>
  </sub>
</p>

<p align="center">
  <sub>Built with 🧡 in Bangkok · If this skill helps you, share it with a teammate.</sub>
</p>
