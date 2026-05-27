# Examples Library

Real-world CLAUDE.md examples with annotations explaining why they work. Use these when:

- Teaching learners (show concrete patterns, not just principles)
- User asks "what does a good CLAUDE.md look like?"
- Demonstrating progressive disclosure or sizing
- Comparing patterns across project types

## Index

| File | Project Type | Lines | Key pattern |
|---|---|---|---|
| `nextjs-app.md` | Web application | ~55 | Standard pattern, balanced |
| `monorepo.md` | Monorepo | ~35 (root) + 25 (sub) | Root + overlay pattern |
| `data-science.md` | Data science / research | ~50 | Domain-specific gotchas |
| `blog-content.md` | Non-code creative writing | ~40 | Voice rules + anti-AI style |
| `marketing-workspace.md` | Non-code marketing/PM | ~45 | "About me" + workflow rules |

## How to read these

Each example has the file content first, then annotations explaining what works and why. The annotations cite principles from the research (pruning test, attention budget, etc.).

Notice across all examples:

1. **None exceed 60 lines** — even the most complex stays lean
2. **Every line is project-specific** — no generic advice
3. **Code style is absent** — handled by linters
4. **Gotchas are concrete** — real incidents, not "watch out for bugs"
5. **Pointers, not content** — long detail moves to docs/

When using examples in teaching, walk through each line and ask: "Does this pass the pruning test?" Build intuition through repetition.
