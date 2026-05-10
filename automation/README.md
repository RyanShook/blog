# Blog publishing automation

A Claude Code scheduled routine that writes and ships one blog post on
weekday mornings, then emails Ryan a copy of the post and the live URL.

## What it does, in order

1. Pulls the latest `main`.
2. Reads `design.md` (brand voice + audience) and the most recent ~5
   posts in `_posts/` to avoid topic repeats.
3. Looks in `_drafts/` first. If any draft has a usable outline, it
   finishes that draft and moves it to `_posts/<DATE>-<slug>.md`.
4. If no usable drafts, it picks a topic from the scope list (see
   `agent-prompt.md`), making sure it hasn't been covered in the last
   30 days.
5. Writes a 800-1,400 word post following the voice rules in
   `agent-prompt.md`. No em dashes, no AI tells, no "robust /
   leverage / unlock," no listicles.
6. Self-reviews against the quality gate. If the post reads generic,
   rewrites the opening + takeaway with sharper specifics.
7. Commits to `main` with message `post: <title>` and pushes. GitHub
   Pages rebuilds and the post is live in ~30 seconds.
8. Emails `shoook@gmail.com` with the live URL, metadata, the full
   post rendered as plain text, and any flags from the run. Email
   is sent via AgentMail (`ryanshook@agentmail.to` → your Gmail).
   The AgentMail API key is stored in the routine config in
   claude.ai (not in this repo).

The full prompt the routine fires lives in [agent-prompt.md](./agent-prompt.md).

## Schedule

- **When:** 5 mornings/week, Monday-Friday.
- **Time:** 7:00 AM America/Chicago.
- **Cron:** `0 7 * * 1-5` with timezone `America/Chicago`.
- **Where it runs:** Anthropic's remote-agent infra (via
  `/schedule create` in Claude Code). Not on Ryan's machine.

## Prerequisites for the routine to actually run

- [ ] Routine is created with `/schedule create` and the cron above.
- [ ] The routine has the AgentMail API key for
      `ryanshook@agentmail.to` set as the env var
      `AGENTMAIL_API_KEY` in its prompt context. Rotate from
      https://agentmail.to if it ever leaks.
- [ ] The routine has git push access to this repo. Default routine
      auth in Claude Code typically handles this, but it should be
      verified on the first manual fire.
- [ ] At least one usable draft in `_drafts/` to seed the first
      runs, or Ryan has accepted that the agent will pick its own
      topics.

## Managing the routine

| Want to do…                   | How                                              |
| ----------------------------- | ------------------------------------------------ |
| See if the routine is on      | `/schedule list`                                 |
| Pause it for a week           | `/schedule update <id>` and disable, or delete   |
| Change the time or cadence    | Edit the cron and run `/schedule update <id>`    |
| Force a run right now (test)  | `/schedule run <id>` (single fire, off-schedule) |
| Change what the agent does    | Edit `agent-prompt.md`, then update the routine  |
| Stop it entirely              | `/schedule delete <id>`                          |

## Watching what it does

After each run the routine emails `shoook@gmail.com` with:

1. The live URL.
2. A metadata block (slug, word count, commit SHA, topic category).
3. The full post as plain text.
4. A review note flagging anything that needs Ryan's eyes (first
   post in a new category, unverified tool reference, hook that
   needed a rewrite, etc.).

If the email doesn't arrive by mid-morning, check `/schedule list`
for the routine's last-run status, then `git log --oneline main`
to see if the commit landed.

## Known risks

- **No human review before publish.** Posts ship to `main` and go
  live before Ryan reads them. The agent prompt has a quality gate
  (re-read once, rewrite if generic), but a wrong tool reference or
  off-brand claim can ship. The morning email is how Ryan catches
  these — fix forward with a follow-up commit if needed.
- **Topic exhaustion.** Daily-ish at 800-1,400 words is aggressive
  for a single domain. After ~2 months the agent will start
  recycling angles. Refilling `_drafts/` periodically with
  human-picked topics is the relief valve.
- **Tool feature drift.** The agent will reference real tools
  (Planning Center, n8n, Claude, Airtable, etc.). It can be wrong
  about specific feature availability. If the morning email's
  metadata flags an unverified tool reference, double-check before
  the post gets traffic.

## File map

```
automation/
├── README.md          ← this file
├── agent-prompt.md    ← exact prompt the routine fires each run
└── topic-queue.md     ← optional human-curated topic ideas the
                          agent can prefer over self-generation
```
