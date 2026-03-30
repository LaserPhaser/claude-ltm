# claude-ltm

Long-term memory for Claude Code. Persist project decisions across sessions so context is never lost.

## The Problem

During large workloads, the context and decisions made at the beginning of a project get lost as Claude Code sessions fill up and context windows rotate. Teams lose track of why choices were made, what was decided, and what changed.

## How It Works

claude-ltm adds a **persistent decision memory** to your project — a structured Markdown file that Claude reads at every session start and updates as decisions are made.

Think of it like human memory:
- A **main file** (`.memory/DECISIONS.md`) holds short decision checkpoints — what you'd recall off the top of your head
- **Detail files** (`.memory/details/`) hold deep-dive context for complex decisions — the memories you dig into when needed

Claude is instructed via `CLAUDE.md` to:
1. **Read** the memory at session start
2. **Update** it when significant decisions are made during work
3. **Compact** before finishing — a final pass to capture anything missed

No hooks, no external services. Just Markdown files and behavioral instructions.

## Install

### 1. Add the marketplace

Add this to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "claude-ltm-marketplace": {
      "source": {
        "source": "git",
        "url": "https://github.com/LaserPhaser/claude-ltm.git"
      }
    }
  }
}
```

### 2. Install the plugin

In any Claude Code session:

```
/plugin install claude-ltm
```

### 3. Initialize in your project

In each project where you want long-term memory:

```
/ltm:init
```

This creates `.memory/DECISIONS.md` and appends instructions to your `CLAUDE.md`.

## Commands

### `/ltm:init`

Initialize the memory system in your project. Creates `.memory/` directory and adds behavioral instructions to `CLAUDE.md`.

### `/ltm:compact`

Manually trigger a compact pass. Claude reviews the current session and extracts any undocumented decisions into `.memory/DECISIONS.md`.

Use this mid-session when you want to checkpoint your decisions.

### `/ltm:save`

Explicitly save a specific decision:

```
/ltm:save chose Stripe over Paddle for payments because of better international coverage
```

Claude formats it and adds it to the appropriate section in the memory file.

## Memory File Format

### Main file (`.memory/DECISIONS.md`)

```markdown
# Project Memory

> Auto-maintained by claude-ltm. Last updated: 2026-03-30

## Architecture

### REST API with Express.js [2026-03-28]
Chose REST over GraphQL because:
- Team has more REST experience
- Client is a simple SPA, no complex nested queries
- Easier to cache at the CDN level

### PostgreSQL for storage [2026-03-30]
Was: MongoDB. Switched because reporting features need JOINs
and the data turned out to be more relational than expected.

## Status
- **Focus**: User authentication flow
- **Blockers**: Waiting on payment provider API key
- **Open**: JWT vs session-based auth?

## Changelog
- 2026-03-30: Switched MongoDB → PostgreSQL
```

**Rules:**
- Each decision gets a `### heading` with date
- Up to ~10 lines of context per entry
- Sections are free-form — Claude creates new ones as needed
- Superseded decisions are updated in-place with change notes

### Detail files (`.memory/details/`)

For complex decisions that need more than 10 lines:

```markdown
---
decision: API Architecture
date: 2026-03-28
status: ACTIVE
---

## Context
[Why this decision was needed]

## Decision
[What was chosen and why]

## Alternatives Considered
- Option B: [why rejected]

## Impact
- [Key consequences]
```

## Uninstall

### Per-project removal

Remove the LTM section from your project's `CLAUDE.md` — delete everything between `<!-- BEGIN claude-ltm -->` and `<!-- END claude-ltm -->`. Optionally delete `.memory/`.

### Global removal

```
/plugin uninstall claude-ltm
```

## Migrating from v0.1

If you previously installed via `npx claude-ltm init`:

1. Delete these files from your project (the plugin system provides them globally now):
   - `.claude/commands/ltm-init.md`
   - `.claude/commands/ltm-compact.md`
   - `.claude/commands/ltm-save.md`
2. Your `.memory/` directory and `CLAUDE.md` instructions are unchanged — no migration needed
3. Install the plugin as described above

## License

MIT
