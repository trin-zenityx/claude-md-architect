# [Project Name]
[One-sentence: what does this project do?]

Stack: [language + version], [framework + version], [database + version], [key libraries].

## Commands
- `[dev command]` — start dev server
- `[test command]` — run tests
- `[test single command]` — run a specific test file
- `[lint command]` — lint check
- `[typecheck command]` — type checking
Run `[lint] && [typecheck] && [test]` before committing.

## Structure
- `[entry folder]/` — [purpose]
- `[lib folder]/` — shared utilities; put new ones here, not in components
- `[test folder]/` — tests mirroring source structure
- `[config folder]/` — configuration files (do not edit lock files directly)

## Rules
- [Convention 1: paired prohibition + direction]
- [Convention 2: imperative with reason if non-obvious]
- [Convention 3]
- All [API responses / function returns] follow shape: `[example]`
- Use `[project-specific helper]`, not `[generic alternative]`
- **IMPORTANT:** [the one critical rule, if any]

## Gotchas
- [Real bug 1: what to remember]
- [Real bug 2: what to remember]
- [Edge case in a specific module]

## See also
- For architecture details: read `docs/architecture.md` when modifying core modules
- For deployment: read `docs/deployment.md` before deploying
- Personal preferences: `CLAUDE.local.md` (gitignored)

<!-- Maintainer notes (stripped from Claude's context, visible to humans only):
     - Created: [date]
     - Last reviewed: [date]
     - Owner: [name]
-->
