This folder contains the source for a Skilled Agent originally built for the Valet runtime. Changes should follow the Skilled Agent open standard.

## What this agent does

Posts a daily finance standup to Slack every weekday at 8am Pacific. Reads Mercury accounts, transactions, and invoices; summarises cash on hand, notable outflows and inflows, and open invoices in a single Slack message. Read-only — never moves money.

## Setup

### Connectors

- **mercury**: Mercury command-line client, used to read account balances (checking, savings, treasury), credit card state, the last 7 days of transactions, and open invoices. Available org-wide as a catalog connector.

### Channels

- **morning-standup** (cron): Fires the daily report. Schedule is `0 15 * * 1-5` UTC (= 8am Pacific during PDT). Triggers `channels/morning-standup.md`.
- **morning-expense-standup** (slack): Per-agent Slack bot for the Valet workspace. Lets `#finance` (or wherever the bot is invited) ask the agent ad-hoc finance questions. Triggers `channels/morning-expense-standup.md`.

### Secrets

- **MERCURY_API_KEY**: Mercury API token with read access to accounts, transactions, and invoices. Set at the org level for `valetdotdev` and reused by every agent that attaches the `mercury` connector.

### External setup

- **Invite the Slack bot to `#finance`**: After deploy, in Slack run `/invite @morning-expense-standup` from `#finance`. The agent falls back to `#money`, `#accounting`, `#ops`, or `#general` if `#finance` doesn't exist.
- **Slack workspace install**: One-time OAuth install of the per-agent Slack app. The CLI prints the install URL after `valet channels create slack` — open it once, approve scopes, then the bot is live in the workspace.

## Known caveats

- **Cron timezone is UTC-only**: The Valet cron channel validator currently rejects IANA timezone names. The schedule `0 15 * * 1-5 UTC` is 8am Pacific while DST is in effect and 7am Pacific outside DST. Adjust to `0 16 * * 1-5` in November when PST returns if you want to keep the post at 8am sharp year-round.
