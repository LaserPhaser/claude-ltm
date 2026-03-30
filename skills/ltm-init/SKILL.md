---
name: ltm:init
description: Initialize the claude-ltm long-term memory system in the current project
---

# Initialize Long-Term Memory

Set up the claude-ltm memory system in this project.

## What to do

1. Check if `.memory/DECISIONS.md` already exists. If it does, inform the user
   that LTM is already initialized and ask if they want to re-initialize
   (which will update CLAUDE.md instructions but NOT overwrite
   existing decisions).

2. Create the following structure:
   ```
   .memory/
   ├── DECISIONS.md    (empty memory with Status + Changelog scaffolding)
   └── details/        (empty directory for future detail files)
   ```

3. Create `.memory/DECISIONS.md` with this content:
   ```markdown
   # Project Memory

   > Auto-maintained by claude-ltm. Last updated: YYYY-MM-DD

   ## Status
   - **Focus**:
   - **Blockers**: None
   - **Open**:

   ## Changelog
   ```

4. Add the LTM instructions section to the project's `CLAUDE.md`. If CLAUDE.md
   doesn't exist, create it. If it exists, append the LTM section at the end.
   Do NOT duplicate if `<!-- BEGIN claude-ltm -->` already exists.

   The instructions to add (wrapped in markers for clean uninstall):
   ```
   <!-- BEGIN claude-ltm -->
   # Long-Term Memory (claude-ltm)

   You have a project decision memory at `.memory/DECISIONS.md`. This file is the
   team's persistent record of significant decisions made during development.

   ## At session start
   - Read `.memory/DECISIONS.md` to understand prior decisions and current status.

   ## During work
   - When you make or discover a significant decision (architecture, tech stack,
     changed approach, business logic, resolved debate), update `.memory/DECISIONS.md`
     immediately.
   - "Significant" = something a teammate joining next week would need to know.
     Routine code changes are NOT decisions. Choosing a database IS.
   - Each entry gets a ### heading with [date], up to ~10 lines of context.
   - If a decision is truly complex (>10 lines), create a detail file in
     `.memory/details/YYYY-MM-DD-<topic>.md` and link to it.
   - When a prior decision is superseded, update its entry in-place. Note what
     changed and why. Update the Changelog section.
   - Check for duplicates before adding — update existing entries, don't repeat.

   ## Before finishing your session
   - Do a compact pass: review this session's work and check if any decisions
     were made but not yet recorded in `.memory/DECISIONS.md`.
   - Update the Status section (current focus, blockers, open questions).
   - Skip if nothing significant was decided.
   <!-- END claude-ltm -->
   ```

5. Confirm to the user what was created and that LTM is now active.

## After initialization

Read `.memory/DECISIONS.md` to confirm it's properly set up. From now on,
follow the LTM instructions in CLAUDE.md for maintaining the decision memory.
