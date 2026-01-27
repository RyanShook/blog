---
title: 'Building My AI Development Environment on Oracle Free Tier'
date: 2026-01-26 17:00:00 -06:00
categories:
  - technology
  - ai
  - cloud
tags:
  - oracle-cloud
  - claude
  - ai-development
  - free-tier
  - mobile-dev
author: Ryan Shook
image:
  path: /assets/img/ai-dev-stack.jpg
  alt: AI Development Stack - Free infrastructure with AI tools and automation
---

I've been experimenting with AI-assisted development and hit a wall: my laptop wasn't enough. Not because of CPU or RAM—but because the moment I closed the lid, I lost my context. Switch devices? Start over. Need to debug something on my phone during lunch? Forget it.

So I built something different: a persistent AI development environment that costs $0/month for infrastructure, accessible from any device, with conversations that never disappear.

**The cost comparison that matters:** A Mac mini M2 Pro costs $1,499 upfront plus $25/month for electricity. Oracle's Always Free tier gives you similar performance—4 ARM cores, 24GB RAM—for $0/month, forever. The only recurring cost? A $20 Claude Pro subscription I was already paying for.

Here's the story of how I built it, what actually works, and why it's changed how I code.

## TL;DR

**What I Built:** An always-on AI development server on Oracle's free tier, accessible from my phone via Happy app, with persistent Claude Code sessions that survive across devices.

**The Secret Sauce:** Free ARM server (4 cores, 24GB RAM) + $20/month Claude Pro + free mobile tools (Termius, Happy iOS) = persistent AI development from anywhere.

**Monthly Cost:** $0 for infrastructure vs $138/month AWS.

**The Win:** Code from anywhere with full context. Start a conversation on my laptop, continue it on my phone days later. Claude remembers everything.

**Time Investment:** 8 hours initial setup, 1 hour/month maintenance.

**Who Should Try:** Developers who want AI-assisted development 24/7, don't mind tinkering, and value never losing context. Not for production workloads.

## The Problem: Laptop Development Doesn't Work for AI

AI coding assistants are incredible. Claude Code, GitHub Copilot, cursor—they're genuinely transformative. But they all share the same fatal flaw: they forget.

Close your laptop? Context gone. Switch from desktop to phone? Start over. Need to review yesterday's architectural discussion? Lost to the ether.

This isn't a bug. It's how terminal and IDE-based tools work. Your conversation lives in RAM, tied to a single device, in a single session. The moment that session ends, so does the AI's memory of your project.

For casual coding, that's fine. But for real work—refactoring a complex codebase, debugging across days, maintaining architectural consistency—it's crippling. I found myself reexplaining context to Claude every morning. "Remember how we decided to use SQLite instead of PostgreSQL?" No, it doesn't remember. Because that conversation died when I closed my laptop.

I tried syncing conversation histories manually. I tried keeping detailed notes. I tried leaving my laptop open 24/7. None of it worked.

I also tried the hosted AI coding platforms—Poke, Manus, Zo Computer. They solve some of these problems: cloud-based environments, persistent sessions, browser access. But the tradeoffs weren't worth it for me. The cost adds up ($30-50/month on top of AI subscription), I lose control over my environment, and I'm locked into their tooling and workflows. The hosted solutions felt like renting someone else's setup instead of owning mine.

What I needed wasn't a better laptop or a hosted IDE. I needed an environment that *never went away*, that I controlled completely, and that cost basically nothing. A server that's always running, with AI sessions that persist indefinitely, accessible from any device.

Claude Code on my laptop was already working great. I just needed it to run somewhere that never shut down.

## The Discovery: Oracle's Actually Free ARM Instances

I'd heard about Oracle Cloud's free tier but assumed it was the usual bait-and-switch: "free" for 12 months, then surprise bills. AWS and GCP burnt me on that before.

Turns out, Oracle's "Always Free" tier is legitimately different:
- **4 ARM Ampere cores** (can split across up to 4 instances)
- **24GB RAM total** (I maxed it in one beefy instance)
- **200GB storage** (boot volumes + block volumes)
- **10TB bandwidth/month** (way more than I'll ever use)
- **Truly always free** - Not a trial. Not "free credits." Just free.

I was skeptical. I signed up expecting to find the catch. Three weeks later, my Oracle bill is still $0.00. The free tier doesn't expire. It doesn't require a credit card (initially). It just... works.

The specs rival my MacBook Pro. ARM Ampere processors are fast, power-efficient, and surprisingly compatible with modern software. Docker images work. Node.js flies. Ubuntu runs native.

For $0/month, I can run a server with more RAM than my laptop, accessible 24/7 from anywhere.

## The Stack: Free Tools, Persistent Context

![AI Development Stack]({{ '/assets/img/ai-dev-stack.jpg' | relative_url }})

Here's what I'm running:

**Core Infrastructure:**
- **Oracle Cloud** (4 ARM cores, 24GB RAM) - The always-on foundation, $0/month
- **Ubuntu 24.04** - ARM64 native, rock solid
- **Tailscale** - Zero-config VPN so I can SSH from anywhere securely

**AI Development Tools:**
- **Claude Code** ($20/month Claude Pro subscription) - The centerpiece, Anthropic's official CLI with full coding capabilities
- **Happy** (free iOS/Android app) - Mobile interface for Claude Code, works surprisingly well on phone
- **Clawdbot** (free, self-hosted) - AI assistant accessible via WhatsApp/Telegram for quick questions

**Persistence & Automation:**
- **n8n** (free, self-hosted) - Workflow automation that can trigger from Claude via MCP
- **Git + rclone** - Nightly backups to GitHub and Google Drive (also free)
- **Termius** (free tier) - SSH client for iOS, how I access the server from my phone

**Total monthly cost:** $20 (just the Claude Pro subscription I was already paying for)

The insight: You don't need expensive infrastructure. You need *persistent* infrastructure. A $0/month server that never stops is worth infinitely more than a $200/month laptop that goes to sleep.

## Building It: A Weekend Project

Setup was straightforward once I had the Oracle instance:
1. **Tailscale** for secure access (`curl install.sh | sh; sudo tailscale up`)
2. **Claude Code** via npm (`npm install -g @anthropic-ai/claude-code`)
3. **Happy** - Install the iOS app, SSH into server, run `happy`, scan QR code
4. **Automatic backups** - One cron job syncing to Google Drive, another exporting Claude conversations to GitHub

I added Clawdbot for quick AI questions from my phone. I set up n8n for workflow automation (blog posts to social media, GitHub monitoring, etc.). I configured automatic security updates.

The entire technical setup took maybe 8 hours spread across a weekend. Most of that was waiting for package installs and reading documentation.

The magic isn't in the complexity. It's in the simplicity. Once running, the system just *works*. Claude sessions persist. Tmux keeps everything alive. Backups run automatically at 3 AM. Security updates install themselves.

**About those conversation backups:** Every night at 3 AM, a cron job exports my Claude Code conversations to GitHub. This solves a critical problem: tmux sessions live in RAM, so if the server reboots (automatic security updates), active conversations disappear. But the nightly exports mean I can always review yesterday's architectural discussions or restore context after a reboot. It's like having perfect memory of every decision and conversation.

I haven't manually intervened in two weeks.

## How I Actually Use It

The setup enables a few workflows that weren't possible before:

**Mobile coding actually works.** I've reviewed PRs and pushed bug fixes from my iPhone using the Happy app. The key is Claude remembers context across devices—I can start a conversation on my laptop, continue on my phone, and pick it back up later without re-explaining anything.

**Context never dies.** SSH into the server, attach to the tmux session, and Claude remembers the conversation from three days ago. No "where was I?" moment. The conversation backups mean even if the server reboots, I can review past discussions and restore context.

**Quick AI queries via WhatsApp.** Clawdbot lets me ask quick technical questions without opening a laptop. It's genuinely useful for checking logs or getting syntax reminders while away from my desk.

**Workflow automation.** n8n can trigger from Claude via MCP, which means I can ask Claude to "run the backup workflow" or "post to social media" and it actually happens.

The real advantage isn't the individual tools—it's that the environment is always there, always warm, and costs $20/month.

## What Changed: The Real Impact

### Context Persistence is a Superpower

The biggest surprise: how much productivity comes from never losing context.

Claude remembers the weird edge case we discussed last week. It remembers why we chose SQLite over PostgreSQL. It remembers the architectural decision to keep services stateless.

This isn't just convenient. It fundamentally changes how I approach complex problems. I don't need to keep the entire system in my head. I don't need extensive notes. Claude remembers for me.

I now start refactoring projects that would've terrified me before. Because I know if I get interrupted—if I need to step away for a day—I can pick up exactly where I left off.

### Mobile Development Actually Works

I was deeply skeptical about coding on a phone. It's one thing to review PRs. It's another to actually write, test, and commit production code.

With Happy + Claude, it works. Not for everything—complex refactoring across 50 files still needs a real keyboard. But for:
- Code review and PR feedback
- Quick bug fixes
- Responding to production issues
- Drafting new features

...my iPhone is surprisingly capable.

The 80/20 rule applies: 80% of developer tasks work fine on mobile with AI assistance. The remaining 20% benefit from large screens and traditional IDEs.

This changes my relationship with work. I'm not "always on call." But if production breaks while I'm out, I can meaningfully contribute from my phone.

### Automation Enables Fearless Experimentation

With nightly backups to Google Drive and GitHub, I experiment more freely.

I'll try risky refactoring. I'll test cutting-edge tools. I'll completely restructure my deployment setup.

Why? Because I can restore everything in 30 minutes if something breaks.

This psychological safety unlocks creativity. I'm not paralyzed by "what if I break it?" Because breaking it is cheap.

## Cost Reality: The Honest Accounting

**What I Actually Pay:**
- Oracle Cloud infrastructure: $0/month (Always Free tier)
- Claude Pro subscription: $20/month (was already paying for this)
- Domain (ryanshook.org): $1.25/month (optional, could use Tailscale hostname)

**Total: $21.25/month**

**What I'm Not Paying:**
- AWS equivalent: ~$138/month (t4g.xlarge + EBS + bandwidth + IP)
- DigitalOcean equivalent: $48/month
- Mac mini server: $1,499 upfront + $25/month electricity

**Savings vs AWS:** ~$1,650/year

The comparison to a Mac mini is particularly interesting. Many developers consider buying a Mac mini to run as a home server. But:
- **Mac mini**: $1,499 upfront, $25/month ongoing, tied to one location, requires physical space and maintenance
- **Oracle free tier**: $0 upfront, $0/month ongoing, accessible from anywhere, Oracle handles hardware failures

Unless you specifically need local hardware (home automation hub, local AI model inference, etc.), the cloud option is objectively better.

## Should You Build This?

**Great fit if you:**
- Want 24/7 AI-assisted development without the cost
- Value context persistence across devices
- Don't mind 8 hours of initial tinkering
- Are comfortable with SSH, basic Linux, and command-line tools
- Want to learn cloud infrastructure, Docker, and networking

**Not a fit if you:**
- Need enterprise support or SLAs
- Want zero maintenance (hosted solutions are easier)
- Have limited technical experience (this has a learning curve)
- Require production-grade reliability
- Don't trust Oracle's "Always Free" commitment

**My verdict:** This is a 9/10 setup for personal development.

**Pros:**
- $0/month infrastructure cost
- Persistent AI environment accessible from any device
- Full control and customization
- Genuinely productive mobile development
- Valuable learning experience

**Cons:**
- Getting the Oracle instance takes patience (capacity constraints)
- Initial setup requires technical comfort
- Not production-ready (by design)
- Oracle's UI is dated compared to AWS

What would make it 10/10? Easier instance provisioning and better Oracle documentation.

## Getting Started: If You Want to Try

Don't try to build everything at once. Start small:

**Week 1:** Spin up the Oracle instance (be patient with capacity), install Claude Code, set up Tailscale for mobile access.

**Week 2:** Try the Happy app on your phone. Configure automatic backups to Google Drive and GitHub.

**Month 2:** Add tools as needed—Clawdbot for messaging, n8n for automation, whatever fits your workflow.

The stack is modular. Each component adds value independently. You don't need everything to benefit from the always-on, persistent environment.

**Key resources:**
- [Oracle Cloud Free Tier](https://www.oracle.com/cloud/free/) - Sign up here
- [Claude Code](https://claude.ai/code) - Anthropic's official CLI
- [Happy Engineering](https://happy.engineering) - Mobile interface
- [Tailscale](https://tailscale.com) - Zero-config VPN

## Final Thoughts

After three weeks living on this setup, I'm not going back.

The cost savings are nice. The technical learning is valuable. But the real win is never losing context.

I work differently now. I'm more willing to tackle complex projects because I know the environment remembers. I'm more productive on mobile because Claude bridges the gap between phone and laptop.

This isn't a replacement for local development. It's a complement. When I need raw compute or multiple monitors, I use my desktop. When I need mobility and persistence, I use the server.

For $20/month (infrastructure is free), I have an AI development environment that's always on, always accessible, and never forgets.

If that sounds valuable to you, it's worth the weekend to build it.

---

## About the Author

**Ryan Shook** is a software developer passionate about AI-assisted development, cloud infrastructure, and building developer tools. He shares his experiments and learnings on his [personal website](https://ryanshook.org) and [GitHub](https://github.com/RyanShook).

This blog post documents a real production setup he uses daily for mobile-first AI development. Connect with him on GitHub or visit his site to see more projects.

---

*This blog post was written with assistance from Claude Code, running on the very infrastructure it describes. Meta, right?*

*Generated with [Claude Code](https://claude.ai/code) via [Happy](https://happy.engineering)*
