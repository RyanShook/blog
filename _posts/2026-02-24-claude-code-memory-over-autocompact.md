---
title: "Claude Code Is Amazing, But AutoCompact Makes It Worse for Real Projects"
date: 2026-02-24 21:00:00 -06:00
categories:
  - technology
  - ai
  - software-engineering
tags:
  - claude-code
  - workflow
  - context
  - project-management
  - ai-tools
author: Ryan Shook
image:
  path: /assets/img/posts/claude-plan-memory-workflow.png
  alt: AutoCompact vs PLAN.md workflow comparison for Claude Code context continuity
---

Claude Code is easily one of the best tools in my workflow right now.

But there is one thing that keeps hurting long-running projects: AutoCompact.

I know why it exists. It looks like the obvious solution for context continuity. The problem is what it actually preserves versus what you need when building real software over multiple sessions.

In my experience, AutoCompact keeps conversation titles and short excerpts, not the full implementation narrative. Even if it kept more text, it still wouldn't capture the lived sequence of decisions as cleanly as a dedicated project plan file that is intentionally maintained.

And that missing trail is exactly where most bugs and regressions hide.

## Why This Matters

When you're shipping code, "what happened" is less important than "why it happened."

You need continuity across the whole chain:

- Failed attempts before the final solution
- Constraint changes after testing or stakeholder feedback
- Refactors that only make sense because of older bugs
- Temporary workarounds that need to be removed later

A compressed summary flattens that. Then the model gives "correct-looking" answers that repeat already-failed ideas.

## What I Recommend Instead

Use this workflow:

1. Memory: Use it for the high-level structure of the project, not every detail.
2. One living `PLAN.md`: This is your source of truth.
3. End-of-session rule: Tell Claude to always update that same file before ending.
4. Next session kickoff: Start with `Claude, continue with PLAN.md`.

Do not create multiple similarly named plan files. One canonical file keeps continuity clean.

## `PLAN.md` Template

In your first session, create `PLAN.md` with this structure:

```md
# PLAN.md

## A) General Work Plan
- Project objective:
- Constraints:
- Architecture notes:

## B) Implementation by Stages
- Stage 1:
- Stage 2:
- Stage 3:

## C) Checklist
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

## D) Progress Percentage
- Overall: 0%

## E) Next Actions
- Next action 1
- Next action 2

## F) Session Handoff Notes
- What changed this session:
- What failed and why:
- What to do first next session:
```

Then add this instruction in your chat:

`At the end of this session, always update PLAN.md with progress, blockers, and next actions.`

## Script 1: Bootstrap a New Project Plan

If you want consistency, use this script once per repo.

```bash
#!/usr/bin/env bash
set -euo pipefail

PLAN_FILE="${1:-PLAN.md}"

if [[ -f "$PLAN_FILE" ]]; then
  echo "$PLAN_FILE already exists."
  exit 0
fi

cat > "$PLAN_FILE" <<'EOF'
# PLAN.md

## A) General Work Plan
- Project objective:
- Constraints:
- Architecture notes:

## B) Implementation by Stages
- Stage 1:
- Stage 2:
- Stage 3:

## C) Checklist
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

## D) Progress Percentage
- Overall: 0%

## E) Next Actions
- Next action 1
- Next action 2

## F) Session Handoff Notes
- What changed this session:
- What failed and why:
- What to do first next session:
EOF

echo "Created $PLAN_FILE"
echo "Kickoff prompt:"
echo "Claude, continue with $PLAN_FILE"
```

## Script 2: Backup Before Major Restructure

Before big refactors, run this first.

```bash
#!/usr/bin/env bash
set -euo pipefail

PROJECT_DIR="${1:-$(pwd)}"
PROJECT_NAME="$(basename "$PROJECT_DIR")"
STAMP="$(date +%Y%m%d-%H%M%S)"
BACKUP_DIR="${HOME}/project-backups"
ARCHIVE="${BACKUP_DIR}/${PROJECT_NAME}-${STAMP}.tar.gz"

mkdir -p "$BACKUP_DIR"
tar -czf "$ARCHIVE" \
  --exclude=".git" \
  --exclude="node_modules" \
  --exclude=".next" \
  -C "$PROJECT_DIR" .

echo "Backup created: $ARCHIVE"
```

## My Practical Split: Claude + ChatGPT

Claude Code is excellent for implementation, debugging, and execution flow.

For creative and visual direction, I still prefer drafting design language/instructions in ChatGPT first, then giving Claude a concrete execution plan.

That's not model tribalism. It's just using the right tool for the job.

## Bottom Line

Claude Code keeps getting better. I use it constantly.

But if you're doing serious multi-session work, don't depend on AutoCompact as your memory system.

Use Memory for project shape, keep one living `PLAN.md`, force end-of-session updates, and make backups before major changes.

That workflow is boring. It also works.
