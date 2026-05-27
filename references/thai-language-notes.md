# Thai Language Notes

Considerations for using CLAUDE.md with Thai-language users, mixed Thai/English content, and teaching Thai students.

## Known CLI Bugs (as of early 2026)

Two confirmed Thai-language bugs in Claude Code CLI around v2.1.20:

- **[Issue #21149](https://github.com/anthropics/claude-code/issues/21149)** — *output* bug: Claude cannot generate Thai text containing sara aa (า). Labeled `bug`, `area:tui`, `area:a11y`, marked as critical/blocking. Open, no current mitigation except downgrade.
- **[Issue #20932](https://github.com/anthropics/claude-code/issues/20932)** — *input* bug: Thai combining vowels (U+0E34–U+0E3A) and tone marks (U+0E47–U+0E4E) display incorrectly when typed in interactive mode. Root cause: input library lacks proper `wcwidth()` for Thai combining marks. Closed as duplicate of an upstream tracked issue.

**Workaround**: Use Claude Code Desktop app, not CLI, for both reading-back and typing Thai content. Or use an external editor (VS Code, Cursor) to write CLAUDE.md, then save to disk — Claude Code reads the file fine, the bug only affects terminal I/O.

**Not affected**:
- Reading existing Thai content from files (file I/O is byte-clean)
- Claude's underlying processing of Thai language (model side is fine)
- Desktop app input/output (different rendering path)

## Mixed Thai/English Pattern (Recommended)

The most effective pattern for Thai users is **English structure + Thai content**:

```markdown
# [ชื่อโปรเจค]
ZenityX LINE Bot สำหรับ admin งานคอร์ส

Stack: Next.js 15, Supabase, LINE Messaging API

## Commands
- `pnpm dev` — start dev server (port 3000)
- `pnpm test` — รัน test suite
- `pnpm typecheck && pnpm lint` — เช็คก่อน commit

## Structure
- `app/api/line/` — webhook handlers ของ LINE
- `lib/rag/` — ระบบ knowledge base + retrieval
- `lib/handoff/` — logic ตัดสินใจส่งต่อ human

## Rules
- ใช้ named exports เท่านั้น ไม่ใช้ default exports
- Validate LINE webhook signature ทุกครั้งก่อนประมวลผล
- Reply Token หมดอายุใน 1 นาที ใช้ push message ถ้าเกิน

## Gotchas
- LINE Switcher API ต้องใช้ channel access token ของ provider ไม่ใช่ channel
- Supabase RLS policy ต้องเปิดสำหรับ service_role key

## See also
- Architecture: อ่าน `docs/rag-architecture.md` ก่อนแก้ retrieval logic
- การจัดการ secrets: `@docs/env-setup.md`
```

### Why this pattern works:

1. **Section headings in English** — Claude has strong English schema matching for "Commands", "Rules", "Structure", "Gotchas". Mixed-language structures with Thai headings sometimes confuse classification.

2. **Domain content in Thai** — Where Thai is the natural language for the team. Project names, business context, gotchas in Thai are easier to maintain.

3. **Technical terms in English** — `Reply Token`, `RLS`, `service_role`, `webhook` — keeping these in English preserves Claude's recognition of them.

4. **Code blocks always in English** — Code is English-centric. Don't try to translate `pnpm dev`.

## Pure Thai Content

If user prefers fully Thai CLAUDE.md, it works but with caveats:

- Token usage will be 2–4× higher per character (UTF-8 encoding)
- Section headings in Thai work, but slightly less reliable Claude classification
- Mix with English technical terms remains essential

Example pure-Thai section:
```markdown
## กฎสำคัญ
- ห้ามใช้ class component ให้ใช้ function component พร้อม hooks
- ทุก API response ต้องมีรูปแบบ `{ data, error, status }`
- ห้ามใช้ `console.log` ให้ใช้ `lib/logger.ts`
```

## Pure English Content

For projects with international team or open-source aim:

- English-only CLAUDE.md
- Comments can be Thai if useful for Thai team members (stripped from context anyway if HTML comment)

Example:
```markdown
## Rules
<!-- กฎเหล่านี้ปรับให้ตรงกับ enterprise standard ของลูกค้า -->
- All API responses follow `{ data, error, status }` shape
- ...
```

HTML comments are stripped by Claude but visible to Thai team members reading the file.

## Teaching Thai Students

When teaching this material to Thai students:

### Pronunciation Notes

First mention of key terms — give Thai-friendly pronunciation:

- CLAUDE.md = "คลอด เอ็ม ดี" or "คลอด ด๊อท เอ็ม ดี"
- Token = "โทเค็น"
- Context = "คอนเท็กซ์"
- Skill = "สกิล"
- Prompt = "พรอมพ์"

### Terms to Keep in English

Always teach these in English (don't translate):
- CLAUDE.md, SKILL.md (file names)
- Token, context, attention
- Pruning test
- Progressive disclosure
- Anti-patterns
- Bash, terminal, CLI
- Worktree, branch, commit
- @import, slash commands

Students will encounter these terms in all documentation and reference material — translating creates confusion.

### Terms to Translate

Concepts that benefit from Thai:
- "Memory" → "ความจำ" or just keep as "memory"
- "Persistence" → "การคงสภาพ"
- "Stale" → "เก่า/ล้าสมัย"
- "Bloat" → "อ้วน/บวม" (informal works well)
- "Refactor" → "ปรับโครงสร้าง"
- "Best practice" → "แนวปฏิบัติที่ดี" or keep English

### Common Confusions to Address

| Confusion | Clarification |
|---|---|
| "CLAUDE.md กับ CLAUDE.local.md ต่างกันยังไง" | CLAUDE.md = shared (commit), CLAUDE.local.md = personal (gitignore) |
| "MEMORY.md คืออะไร" | Claude เขียนเอง อัตโนมัติ ต่างจาก CLAUDE.md ที่คนเขียน |
| "ทำไมไม่ใช่ system prompt" | จริงๆ ถูกส่งเป็น user message แต่ทำงานคล้าย system prompt |
| "Token คือตัวอักษรใช่ไหม" | คล้ายๆ ตัวอักษร แต่อังกฤษ 1 token ≈ 4 chars ไทย 1 token ≈ 1–2 chars |
| "ทำไมไม่ใส่ทุกอย่างใน CLAUDE.md" | ยิ่งใส่เยอะ Claude ยิ่งไม่ตาม ใส่แค่ที่จำเป็น |

### Suggested Workshop Structure (90 min)

```
0:00–0:10  Introduction
            - CLAUDE.md คืออะไร (concept)
            - ทำไมต้องเรียน (relevance)

0:10–0:25  Layer 1: How it works
            - File location
            - Auto-loading mechanism
            - Demo: เปิด session ใหม่ → Claude อ่าน CLAUDE.md อัตโนมัติ

0:25–0:40  Layer 2: Why it matters (attention budget)
            - Token cost
            - Attention degradation (gradient, not a cliff — show IFScale data)
            - The pruning test (สำคัญที่สุด)
            - แบบฝึก: ดูตัวอย่าง 10 บรรทัด เลือกที่ควรเก็บ

0:40–0:55  Layer 3: Anatomy
            - Why/What/How structure
            - Template walkthrough
            - แบบฝึก: เขียน CLAUDE.md 30 บรรทัดสำหรับ project ของตัวเอง

0:55–1:10  Layer 4: Anti-patterns
            - Top 5 anti-patterns พร้อมตัวอย่าง bad → good
            - Audit ตัวอย่าง bloated CLAUDE.md ด้วยกัน

1:10–1:25  Hands-on
            - นักเรียนเขียน CLAUDE.md สำหรับ project จริง
            - Pair review

1:25–1:30  Closing
            - Resources
            - Next steps
```

## Translating Master Prompts to Thai

If translating extract mode workflow to Thai, keep these in English:
- Phase names ("Phase 1: Memory Dump")
- Tool/command names
- File names
- Technical category labels (A, B, C, ...)

Translate:
- Instructions and explanations
- Examples within categories
- Quality checklist items

This balances accessibility (Thai readability) with technical accuracy (recognizable terms).

## Common Thai-Specific Gotchas

### Naming Files in Thai

Don't use Thai characters in filenames. Use English with Thai content inside:

✅ `docs/architecture.md` (with Thai content)
❌ `docs/สถาปัตยกรรม.md` (Thai filename, harder to reference)

Reason: Thai characters in paths cause issues across different shells, git on Windows, etc.

### Thai Quotation Marks

In Markdown, use English quotes (`"` and `'`), not Thai-style quotes (`「」` or `『』` or `“”`). Some markdown parsers misinterpret typographic quotes.

### Mixed Content in Lists

When mixing Thai and English in a bullet list, English first usually flows better:

✅ `- pnpm dev — รัน dev server (port 3000)`
❌ `- รัน dev server (port 3000) — pnpm dev`

The technical term anchors the eye, Thai description follows. This is a style preference, not a hard rule.

## Resources for Thai Users

- Official Anthropic docs available in English at code.claude.com/docs
- Thai community AI groups: search Facebook for "AI Builder Thailand", "Vibe Coding ไทย"
- For Thai-language tutorials, ZenityX courses cover this material in Thai
