# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a TypeScript course transcript archive, not a runnable codebase. It contains lecture transcripts from Maximilian Schwarzmüller's TypeScript course (339 lectures), stored as plain-text files. There is no build system, no package.json, and no TypeScript source to compile.

## Structure

- `lectures.txt` — master table of contents, one lecture per line in format `N. Lecture Title`
- `lectures/` — individual transcript files, one per lecture, named `N.Lecture_Title_With_Underscores.txt`
- `lectures.txt.bak` — backup of the TOC

## Lecture Coverage

The course covers TypeScript from basics to advanced topics, grouped into these major sections:

| Lectures | Topic |
|----------|-------|
| 1–9 | Course intro & setup |
| 10–49 | TypeScript essentials (types, inference, generics intro, enums, tuples) |
| 50–66 | Modern JavaScript features with TypeScript (arrow fns, spread, destructuring) |
| 67–99 | Classes, interfaces, advanced types (type guards, discriminated unions, overloads) |
| 100–109 | Generics in depth |
| 110–117 | Generic linked list project |
| 118–132 | Advanced type utilities (typeof, keyof, mapped types, conditional types, infer) |
| 133–161 | Decorators (ECMAScript decorators + experimental) |
| 162–181 | Full project: drag-and-drop app |
| 182–213 | Modules, namespaces, build tools (Webpack, Vite) |
| 214–231 | Third-party library types (.d.ts, @types, Zod) |
| 232–245 | React + TypeScript |
| 246–257 | Node.js + Express + TypeScript |
| 258–339 | Legacy lectures (older recordings of earlier topics) |

## Common Tasks

**Find lectures on a specific topic:**
```bash
grep -i "keyword" lectures.txt
```

**Read a specific lecture transcript:**
```bash
# Files use underscores for spaces in the title
cat lectures/103.Creating_Using_a_Generic_Type.txt
```

**List all lectures in a section (e.g., decorators section 133–161):**
```bash
sed -n '133,161p' lectures.txt
```

**Check which lecture files are present vs expected:**
```bash
find lectures/ -name "*.txt" | wc -l  # should be 339
```


<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:ca08a54f -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files

## Session Completion

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   bd dolt push
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds
<!-- END BEADS INTEGRATION -->
