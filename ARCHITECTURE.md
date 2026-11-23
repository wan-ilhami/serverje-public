# Architecture

```
 browser                      panel (Next.js)                     your server (Ubuntu)
 ┌──────────────┐  HTTPS   ┌─────────────────────────┐  HTTPS   ┌──────────────────────┐
 │ dashboard    │◄────────►│ pages and server actions│◄─────────│ agent (bash + curl)  │
 │ websites,    │          │ agent API, MCP endpoint │ polling  │ runs one fixed script│
 │ backups, VMs │          │ cron tick               │          │ per task kind        │
 └──────────────┘          └───────────┬─────────────┘          └──────────────────────┘
                                       │
                          Postgres · S3/R2 backups · Proxmox API · SMTP
```

## Pull, not push

The agent polls the panel every few seconds with CPU, memory and disk figures, and the answer carries the next
queued task. The server never opens an inbound port, and the panel never logs in over SSH. The same design lets the
panel run on a serverless host, because nothing long-running lives in it.

## Tasks

- Each task kind is one fixed shell script shipped with the agent. User input is passed as data, never spliced into
  a script, and the scripts are tested with hostile input.
- Output streams back while a task runs. A task that runs past its limit is marked failed so it can be retried.
- Secrets travel as task parameters that are dropped from the panel once the agent has taken the task.
- The agent updates itself: when its script changes, the panel queues an update task on the next check-in.

## Security

- Agent and session tokens are stored only as SHA-256 hashes. Stored secrets are encrypted at rest.
- Every mutation checks the team role on the server; hidden buttons are not the control.
- Alert channels accept only the exact provider hosts, and uptime monitors refuse private addresses.
- Proxmox connections pin the TLS fingerprint instead of turning verification off.
- The AI connection uses hashed bearer tokens, redacts output, rate-limits calls, and holds every write action for
  human approval.
- Sign-in, reset and sign-up are rate-limited, and the reset answer never reveals whether an account exists.

## Background work

A cron tick checks monitors and SSL certificates, opens and closes incidents, queues daily backups and moves virtual
machine creation forward one step at a time. Each step is claimed with one atomic database update so it runs once.

## Data

Postgres holds teams, servers, sites, tasks, incidents, backups and plans. Local development runs Postgres, a mail
catcher and an S3-compatible store in Docker; end-to-end tests play the agent over HTTP against a mock Proxmox.
