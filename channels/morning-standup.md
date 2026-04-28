# Morning Expense Standup (cron)

It is 8am Pacific on a weekday. Run the daily finance standup.

## Step 1: Pull balances from all three account types

Mercury splits accounts across three resources. Hit all three —
`accounts` alone misses treasury and credit, where most of the
real money lives.

1. `mercury accounts list` — depository (checking, savings).
   Capture each account's `id`, `name`, `kind`, and
   `availableBalance`.
2. `mercury credit list` — credit accounts. Capture each
   account's `id` and `availableBalance` (negative value =
   outstanding balance drawn against the line).
3. `mercury treasury list` — treasury / money market accounts.
   Capture each account's `id` and `availableBalance`.

## Step 2: Pull the last 7 days of transactions

Compute the date 7 days ago in `YYYY-MM-DD` format. Then:

1. Collect all depository account IDs from Step 1 #1 and all
   credit account IDs from Step 1 #2.
2. Call `mercury transactions list --start <YYYY-MM-DD>` with
   one `--account-id <id>` flag per ID (the flag is repeatable).
   This single call returns both debit and credit-card
   transactions across all those accounts.
3. For each treasury account ID from Step 1 #3, call
   `mercury treasury transactions --treasury-id <id>` and keep
   only entries within the 7-day window.

## Step 3: Pull open invoices

`mercury invoices list` — keep entries whose status is **not**
`Paid` or `Cancelled` (i.e. `Sent`, `Overdue`, etc.). Sum the
amounts and count them.

If a Mercury subcommand or flag isn't right, run
`mercury <resource> --help` to discover correct usage. Don't
loop on the same failed command.

## Step 4: Compose the briefing

Per the SOUL.md format:

- **Cash on hand** = depository total + treasury total. One
  bullet per bucket (Checking, Savings, Treasury) plus a total.
- **Credit outstanding** = abs(credit availableBalance). If $0,
  say so explicitly.
- **Notable outflows** = top 3 by amount from the last 7 days
  across depository + credit, plus anything over $5,000. Include
  payment method (e.g. "Credit Card ••2878").
- **Notable inflows** = same logic.
- **Open invoices** = one line: total $ + count.

## Step 5: Post to Slack

1. Try `#finance` first. Fall back in order: `#money`,
   `#accounting`, `#ops`, `#general`.
2. Lead with today's date (Pacific timezone).
3. Slack mrkdwn formatting (bold headers, bullets, currency with
   thousands separators).
4. No `@channel` / `@here` mentions.
5. End the run after the post succeeds. Do not follow up.

If any data source is unavailable, post a partial briefing and
note the gap explicitly (e.g. "Treasury balances unavailable").
