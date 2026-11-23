# serverje

**A control panel for Ubuntu servers you run yourself.**

serverje manages the servers you already have: rented VPSes, old PCs at home, or virtual machines on your own
Proxmox cluster. You paste one command on a server and a small agent connects it to the panel. From then on the
panel shows CPU, memory and disk, and runs tasks such as creating a website or taking a backup. There is no open
port on the server and no SSH from the panel. The interface is English only and uses plain words ("Websites",
"Scheduled jobs"), not sysadmin jargon.

## Screenshots

Screenshots are to come.

## What's inside

- **Servers and websites** — static, PHP, WordPress, Laravel, Node.js and Docker apps. Free SSL, `www` aliases,
  Cloudflare Tunnel for servers behind a home router, deploy from Git (private repositories too) or from an image,
  health-checked releases with rollback, a push webhook and environment variables.
- **Looking after them** — uptime monitors, incidents, alerts by email, Telegram, Slack and Discord, SSL expiry
  warnings, disk and memory alerts, usage charts, logs, firewall rules, OS updates and restarts.
- **Backups** — daily backups to your own S3 or Cloudflare R2 bucket, restore, retention, and SSL keys only if you
  opt in.
- **Teams and plans** — owner, admin and member roles, invites, and per-team plans with limits.
- **Kubernetes** — join several servers into a k3s cluster and deploy and scale apps on it.
- **Virtual machines** — connect a Proxmox VE cluster, create Ubuntu VMs from a template and have them join the
  panel by themselves.
- **AI connection** — Claude Code, Cursor or VS Code can read server status through MCP, and deploys, backups and
  log reads wait for your approval in the panel.

## Architecture

See [`ARCHITECTURE.md`](./ARCHITECTURE.md).

## What this repository is

This is the **public** showcase of serverje. The application code lives in a private repository. This repo holds
only the product description, the architecture overview, the version table and screenshots — no source code, keys,
tokens, internal URLs or personal data.
