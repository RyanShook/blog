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

I was about to buy a $1,499 Mac mini M2 Pro to run AI agents on an always-on server. Instead, I used Oracle's free tier and got 4 ARM cores with 24GB RAM for $0/month.

Browser-based AI tools like ChatGPT and Claude don't persist context between sessions or across devices. Openclaw runs on your own server with full file system access, remembers conversations across reboots, and connects to multiple messaging platforms.

This guide walks through setting up Openclaw on Oracle's Always Free tier.

## The Setup: Step-by-Step

Here's exactly how I built it. Total time: ~3 hours. Monthly cost: $0 for infrastructure + $20 Claude Pro subscription.

### Step 1: Sign Up for Oracle Free Tier

1. Go to [oracle.com/cloud/free](https://www.oracle.com/cloud/free/)
2. Click "Start for free"
3. Create account (requires credit card for verification, but it's never charged)
4. Complete verification

Oracle's free tier doesn't expire. These resources remain available as long as the account is active.

### Step 2: Spin Up the Max Free Instance

In the Oracle Cloud Console:

1. **Compute > Instances > Create Instance**
2. **Name:** ai-server (or whatever you want)
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

**Capacity issues?** If you get "out of capacity," try a different availability domain or wait 24 hours and retry.

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

Tailscale provides secure remote access without exposing SSH to the public internet:

```bash
# Install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh

# Start and authenticate
sudo tailscale up

# Visit the auth URL it prints, log in, approve device
```

You can now SSH via the Tailscale IP (100.x.x.x) instead of the public IP.

### Step 5: Install Node.js

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
rclone sync ~/ gdrive:server-backups \
  --exclude node_modules/** \
  --exclude .git/objects/** \
  --exclude .cache/** \
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

### Step 11: Enable Heartbeat Monitoring (Optional)

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

Openclaw will now run checks every 30 minutes and send alerts when needed.

## What You Get

**Cost:** $0/month infrastructure + $20/month Claude Pro subscription

**Specs:**
- 4 ARM Ampere cores
- 24GB RAM
- 200GB storage
- Always-on AI assistant via Telegram
- Persistent conversations across sessions
- File system and shell access
- Automated backups

## Next Steps

Additional features you can add:

- More channels: WhatsApp, Slack, Discord, iMessage
- Gmail/Calendar integration via OAuth
- Receipt processing with Google Drive storage
- Workflow automation with n8n
- Mobile coding via Happy app (Claude Code on iOS/Android)

The setup is modular. Start with Openclaw + Telegram and add other components as needed.

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

## Summary

Oracle's free tier provides 4 ARM cores and 24GB RAM at no cost. Openclaw offers persistent AI conversations with file system access, accessible via messaging platforms like Telegram.

Setup takes approximately 3 hours. The core components (Oracle instance + Openclaw + Telegram) provide the base functionality. Additional features can be added later.

Total monthly cost: $20 for Claude Pro subscription. Infrastructure remains free.

---

**Resources:**
- [Oracle Cloud Free Tier](https://www.oracle.com/cloud/free/)
- [Openclaw Documentation](https://openclaw.ai)
- [Telegram BotFather](https://t.me/botfather)
- [Tailscale Setup](https://tailscale.com)
