---
title: 'Running Openclaw for Free on Oracle Free Tier'
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
  - openclaw
author: Ryan Shook
image:
  path: /assets/img/ai-dev-stack.jpg
  alt: AI Development Stack - Free infrastructure with AI tools and automation
---

I was about to buy a $1,499 Mac mini M2 Pro just to have an always-on server for running AI agents. Then I realized: why spend $1,500 when I can get the same thing for free?

**The problem with ChatGPT and Claude:** They forget. Close the browser, lose context. Switch devices, start over. They're amazing tools, but they live in isolated sessions with no memory, no persistence, and no access to your actual environment.

**Openclaw changes everything.** It's not just a chatbot—it's a full AI assistant with persistent context, file system access, browser control, and workflow automation. Most importantly, it runs on *your* infrastructure, accessible from any device, and remembers everything.

And thanks to Oracle's Always Free tier, that infrastructure costs $0/month.

## Why Openclaw > ChatGPT/Claude

**Persistence:** Conversations survive server reboots, device switches, and weeks of inactivity. Ask a question today, pick up the conversation next week.

**Context:** Full access to your files, git repos, and running services. It's not hallucinating about your codebase—it's reading it.

**Always-on:** The server never sleeps. Set up heartbeat monitoring and it proactively checks your email, calendar, or deployment status.

**Multi-channel:** Chat via Telegram, WhatsApp, iMessage, Slack, Discord—wherever you already are. No app switching.

**Control:** Self-hosted, open source, runs on infrastructure you own. No vendor lock-in, no usage limits beyond your Claude API subscription.

This isn't "ChatGPT but self-hosted." It's an entirely different paradigm—a persistent AI agent with shell access, living on a server you control.

## The Setup: Step-by-Step

Here's exactly how I built it. Total time: ~3 hours. Monthly cost: $0 for infrastructure + $20 Claude Pro subscription.

### Step 1: Sign Up for Oracle Free Tier

1. Go to [oracle.com/cloud/free](https://www.oracle.com/cloud/free/)
2. Click "Start for free"
3. Create account (requires credit card for verification, but it's never charged)
4. Complete verification

**Important:** Oracle's free tier is "Always Free" - no trial period, no expiration. Once created, these resources are yours forever.

### Step 2: Spin Up the Max Free Instance

In the Oracle Cloud Console:

1. **Compute > Instances > Create Instance**
2. **Name:** openclaw-server (or whatever you want)
3. **Image:** Ubuntu 24.04 (Canonical-Ubuntu-24.04-aarch64)
4. **Shape:** 
   - Click "Change Shape"
   - Select "Ampere" (ARM)
   - Choose VM.Standard.A1.Flex
   - Set **4 OCPUs** and **24GB RAM** (maxes out free tier)
5. **Networking:** Use default VCN (creates automatically)
6. **SSH Keys:** 
   - Generate a key pair (Download private key!)
   - Or paste your existing public key
7. **Boot Volume:** 200GB (max free tier)
8. Click **Create**

Wait 3-5 minutes for the instance to provision. Note the public IP address.

**Capacity issues?** Oracle's free ARM instances are popular. If you get "out of capacity," try a different availability domain or wait 24 hours and retry. It's worth the wait.

### Step 3: Initial Server Access

From your terminal:

```bash
# SSH into your new server
ssh -i /path/to/your/private-key.pem ubuntu@YOUR_PUBLIC_IP

# Update packages
sudo apt update && sudo apt upgrade -y

# Install essentials
sudo apt install -y git curl wget build-essential
```

### Step 4: Install Tailscale (Secure Remote Access)

Tailscale creates a private VPN so you can SSH from anywhere without exposing ports:

```bash
# Install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh

# Start and authenticate
sudo tailscale up

# Visit the auth URL it prints, log in, approve device
```

Now you can SSH via Tailscale IP (100.x.x.x) instead of the public IP. Much more secure.

### Step 5: Install Node.js (for Openclaw)

```bash
# Install fnm (Fast Node Manager)
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc

# Install latest Node LTS
fnm install --lts
fnm use lts-latest
fnm default lts-latest

# Verify
node --version  # Should show v22.x.x or higher
```

### Step 6: Install Claude CLI

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Authenticate (opens browser for OAuth)
claude auth login

# Test it
claude chat "Hello from my new server!"
```

### Step 7: Security Hardening

**Set up fail2ban (blocks brute-force SSH attacks):**

```bash
sudo apt install -y fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

**Configure automatic security updates:**

```bash
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
# Select "Yes" to enable automatic updates
```

**Lock down file permissions:**

```bash
# Your home directory should be private
chmod 700 ~

# SSH keys should be locked down
chmod 600 ~/.ssh/authorized_keys
```

### Step 8: Set Up Backups and Self-Healing

**Install rclone for Google Drive backups:**

```bash
# Install rclone
sudo apt install -y rclone

# Configure Google Drive
rclone config
# Follow prompts: "n" for new remote, name it "gdrive", choose "drive" type

# Create backup script
cat > ~/backup-to-gdrive.sh << 'EOF'
#!/bin/bash
rclone sync ~/openclaw-workspace gdrive:openclaw-backups \
  --exclude node_modules/** \
  --exclude .git/objects/** \
  --log-file ~/backup.log
EOF

chmod +x ~/backup-to-gdrive.sh
```

**Set up automated backup cron job:**

```bash
# Edit crontab
crontab -e

# Add this line (runs backup daily at 3 AM):
0 3 * * * /home/ubuntu/backup-to-gdrive.sh
```

**Self-healing: Auto-restart Openclaw on failure:**

```bash
# Create watchdog script
cat > ~/openclaw-watchdog.sh << 'EOF'
#!/bin/bash
if ! pgrep -f "openclaw.*gateway" > /dev/null; then
    cd ~ && openclaw gateway --daemon
    echo "$(date): Openclaw gateway restarted" >> ~/watchdog.log
fi
EOF

chmod +x ~/openclaw-watchdog.sh

# Add to crontab (checks every 5 minutes)
crontab -e
# Add line:
*/5 * * * * /home/ubuntu/openclaw-watchdog.sh
```

### Step 9: Install Openclaw

```bash
# Install globally
npm install -g openclaw@latest

# Run onboarding wizard
openclaw onboard

# Follow prompts:
# - Model: Select Claude (use your Claude Pro auth)
# - Workspace: Accept default (~/ or create ~/openclaw-workspace)
# - Channels: Select Telegram (easiest to set up)
# - Skills: Install recommended (or skip for now)
```

The wizard will walk you through everything. Most important: the Telegram setup.

### Step 10: Configure Telegram

**Create a Telegram bot:**

1. Open Telegram, search for [@BotFather](https://t.me/botfather)
2. Send `/newbot`
3. Choose a name (e.g., "My Openclaw Bot")
4. Choose a username (e.g., `my_openclaw_bot`)
5. Copy the API token BotFather gives you

**Add token to Openclaw:**

During `openclaw onboard`, when asked for Telegram token, paste it.

Or manually add to `~/.openclaw/openclaw.json`:

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "botToken": "YOUR_TOKEN_HERE"
    }
  }
}
```

**Start the gateway:**

```bash
openclaw gateway --daemon
```

**Test it:**

1. Find your bot in Telegram (search for the username you created)
2. Send `/start`
3. Send a message like "What's the server uptime?"

If it responds, you're done!

### Step 11: Enable Heartbeat Monitoring (Optional but Awesome)

Edit `~/openclaw-workspace/HEARTBEAT.md`:

```markdown
# Check system status
- Run: `uptime` and `df -h` to check server health
- If disk usage > 80%, alert me

# Check for important emails (if Gmail configured)
- Run email check script
- Alert if unread from specific senders

# Return HEARTBEAT_OK if nothing needs attention
```

Configure heartbeat interval in `~/.openclaw/openclaw.json`:

```json
{
  "agents": {
    "defaults": {
      "heartbeat": {
        "every": "30m"
      }
    }
  }
}
```

Now Openclaw checks in every 30 minutes and can proactively alert you.

## What You Get

**Total cost:** $0/month for infrastructure + $20/month Claude Pro

**What you have:**
- Always-on AI assistant accessible from any device
- Persistent context that survives reboots and device switches
- Full file system and command access
- Automated backups and self-healing
- Multi-channel access (Telegram, plus add more later)
- 4 ARM cores and 24GB RAM to run whatever else you need

**Compared to buying a $1,500 Mac mini to run AI agents:** You just saved $1,500 and got more RAM.

## Next Steps

Once the basics are running:

**Add more channels:** WhatsApp, Slack, Discord, iMessage (requires setup)

**Gmail/Calendar integration:** OAuth setup for email and calendar automation

**Receipt processing:** Forward receipts → auto-saved to Google Drive

**Workflow automation:** Install n8n for complex automation pipelines

**Mobile coding:** Install Happy app for Claude Code on iOS/Android

The beauty of this setup is modularity. Start with the core (Openclaw + Telegram) and add features as you need them.

## Troubleshooting

**"Out of capacity" error on Oracle?** 
- Try different availability domains
- Check again in 24 hours (capacity fluctuates)
- Free tier instances are competitive—be patient

**Openclaw gateway won't start?**
- Check logs: `openclaw logs --follow`
- Verify Node version: `node --version` (need 22+)
- Check port conflicts: `sudo ss -tulpn | grep 18789`

**Telegram bot not responding?**
- Verify gateway is running: `ps aux | grep openclaw`
- Check token in `~/.openclaw/openclaw.json`
- Test token: `curl https://api.telegram.org/bot<TOKEN>/getMe`

**Lost SSH access?**
- Use Oracle Cloud Console > Instance > Console Connection
- Or reboot instance from console

## Key Takeaways

**Don't buy a Mac mini just to run AI agents.** Oracle's free tier gives you better specs for $0.

**Openclaw > ChatGPT/Claude** when you need persistence, context, and system access.

**The setup takes ~3 hours** but you get an always-on AI environment that costs nothing to run.

**Start simple.** Core setup (Oracle + Openclaw + Telegram) gets you 80% of the value. Add complexity only when you need it.

**This is infrastructure you own.** No vendor lock-in, no usage caps, complete control.

---

**Resources:**
- [Oracle Cloud Free Tier](https://www.oracle.com/cloud/free/)
- [Openclaw Documentation](https://openclaw.ai)
- [Telegram BotFather](https://t.me/botfather)
- [Tailscale Setup](https://tailscale.com)
