---
title: 'Building My AI Development Environment on Oracle Free Tier'
date: 2026-01-26 23:05:00 -06:00
categories:
  - technology
  - ai
  - cloud
tags:
  - oracle-cloud
  - claude
  - clawdbot
  - happy-engineering
  - ai-development
  - free-tier
author: Ryan Shook
---

I've been experimenting with AI development tools and wanted a persistent environment where I could run Claude Code CLI, Clawdbot (my personal AI assistant), and the Happy app — all without paying for hosting. Here's how I built a complete AI development stack on Oracle Cloud's free tier.

## The Stack

- **Infrastructure:** Oracle Cloud Free Tier (ARM64)
- **OS:** Ubuntu Linux
- **AI Tools:**
  - [Claude Code CLI](https://claude.ai/code) - Anthropic's official CLI for Claude
  - [Clawdbot](https://clawdbot.com) - Personal AI assistant with multi-channel support
  - [Happy Engineering](https://happy.engineering) - AI-powered development workflow automation
- **Additional Tools:**
  - n8n workflow automation
  - Cloudflare Tunnels for secure access
  - tmux for session management
- **Cost:** $0/month

## Why This Setup Works

**Oracle Free Tier is Generous**
- 4 ARM-based Ampere A1 cores (24GB RAM)
- 200GB block storage
- 10TB monthly bandwidth
- Always free (not a trial)

**Everything Runs 24/7**
- Persistent development environment
- No cold starts
- Always accessible via Cloudflare Tunnels

**AI-First Development**
- Claude Code for interactive coding sessions
- Clawdbot for personal assistance across messaging platforms
- Happy app for workflow automation
- n8n for building custom automations

## The Setup Process

### 1. Oracle Cloud Infrastructure

I set up an Oracle Cloud account and spun up an ARM-based compute instance using the Always Free tier. The ARM architecture is perfect for this — it's efficient and powerful enough to run multiple Node.js applications simultaneously.

### 2. Claude Code CLI

Claude Code is Anthropic's official CLI that provides an interactive coding assistant directly in the terminal. It's like having a pair programmer available 24/7.

Installation:
```bash
npm install -g @anthropic-ai/claude-code
```

I created a custom YOLO mode configuration that enables Claude to operate with maximum autonomy. The key is balancing speed with safety — Claude makes decisions independently but always asks before deletions.

### 3. Clawdbot - Personal AI Assistant

Clawdbot is a personal AI assistant that runs on my infrastructure and connects to the messaging platforms I already use. Unlike hosted solutions, I control everything.

Features I'm using:
- **Multi-channel support**: WhatsApp, Telegram, Slack, Discord, and more
- **Voice capabilities**: Speak and listen on macOS/iOS/Android
- **Extensible skills system**: Custom commands and automations
- **Self-hosted**: Full control over data and functionality

The setup was straightforward using their onboarding wizard:
```bash
npm install -g clawdbot@latest
clawdbot onboard --install-daemon
```

### 4. Happy Engineering Integration

The Happy app provides workflow automation and integrates seamlessly with Claude Code. It runs in the background via tmux alongside Claude:

```bash
tmux new-session -d -s happy bash -l -c 'happy & claude'
```

This creates a persistent environment where both tools are always running and ready to assist.

### 5. n8n Workflow Automation

I installed n8n for building custom workflow automations. It's accessible via Cloudflare Tunnel at a custom subdomain and provides a visual workflow builder for connecting various services and APIs.

### 6. Cloudflare Tunnels

All services are exposed securely through Cloudflare Tunnels — no need to open firewall ports or manage SSL certificates. I'm using `cloudflared` to create a tunnel for n8n and any other web services.

## Automation & Backups

Everything is backed up automatically via cron jobs:

| What | When | Script |
|------|------|--------|
| Conversations | Nightly 3 AM CST | `sync-conversations.sh` |
| Scripts | Daily 9 AM UTC | `sync-scripts.sh` |
| Google Drive Backup | Daily 8 AM UTC | `backup-to-gdrive.sh` |

All scripts are version controlled in a Git repository, so I can recreate the entire environment from scratch if needed.

## My Development Workflow

1. **SSH into the Oracle instance** (or use Claude Code remotely)
2. **Attach to tmux session** with Happy and Claude running
3. **Chat with Claude** for coding assistance and problem-solving
4. **Message Clawdbot** on WhatsApp/Telegram for quick queries
5. **Trigger n8n workflows** for automated tasks
6. **Everything persists** — sessions continue even when disconnected

## What I Learned

**Oracle Free Tier is Underrated**
The 24GB RAM and 4 ARM cores are more than enough for running multiple Node.js applications. The network performance is excellent, and the free tier truly is "always free" — not a trial that expires.

**ARM Architecture is Powerful**
Modern ARM processors are incredibly efficient. The Ampere A1 cores handle everything I throw at them without breaking a sweat.

**AI Tools Need Persistent Environments**
Having Claude Code and Clawdbot always running means I can pick up conversations where I left off. Context persistence is crucial for productive AI-assisted development.

**Automation is Essential**
The automated backups and script syncing give me confidence to experiment freely. If something breaks, I can restore from backups or rebuild from version-controlled scripts.

**Cloudflare Tunnels are Magic**
No port forwarding, no SSL certificate management, no firewall rules. Just install `cloudflared`, create a tunnel, and services are securely accessible with a custom domain.

## Performance Notes

Resource usage on the Oracle ARM instance:
- **Claude Code:** Minimal (runs only when actively coding)
- **Clawdbot:** ~200-400MB RAM when idle
- **Happy app:** ~100-200MB RAM
- **n8n:** ~300-500MB RAM
- **System overhead:** ~1-2GB RAM

With 24GB total, there's plenty of headroom for additional services.

## Security Considerations

- All services behind Cloudflare Tunnels (no direct exposure)
- SSH key authentication only (no password auth)
- Regular automated backups to Google Drive
- Environment variables for sensitive credentials
- Git repository for infrastructure as code

## Cost Analysis

**Oracle Free Tier:**
- Compute: $0
- Storage: $0
- Network: $0

**Domain (optional):**
- ryanshook.org via Namecheap: ~$15/year

**Total monthly cost:** $0 (excluding domain)

Compare this to running equivalent infrastructure on AWS, Azure, or even a VPS — you'd easily be paying $50-100/month for similar resources.

## Future Plans

- Add more Clawdbot skills for home automation
- Build custom n8n workflows for content creation
- Experiment with additional AI models via Clawdbot
- Create a web interface for Claude Code conversations
- Integrate with GitHub Actions for CI/CD

## Final Thoughts

This setup has fundamentally changed how I develop and interact with AI tools. Having a persistent, powerful environment that costs nothing to run removes all friction from experimentation.

If you're interested in AI-assisted development and want an always-on environment without monthly costs, Oracle Cloud's free tier is an excellent foundation.

**Resources:**

- [Claude Code](https://claude.ai/code) - Anthropic's official CLI
- [Clawdbot](https://clawdbot.com) - Personal AI assistant
- [Happy Engineering](https://happy.engineering) - AI development workflows
- [Oracle Cloud Free Tier](https://www.oracle.com/cloud/free/)
- [Cloudflare Tunnels](https://www.cloudflare.com/products/tunnel/)
- [n8n](https://n8n.io/) - Workflow automation

---

*Want to discuss this setup or share your own AI development environment? Find me on [GitHub](https://github.com/RyanShook) or [connect via my site](https://ryanshook.org).*
