# Morning Expense Standup

A daily [Mercury](https://mercury.com) finance standup posted to Slack every weekday at 8am Pacific. Pulls account balances (checking, savings, treasury, credit), the last 7 days of transactions, and open invoices, then writes a concise update with cash on hand, notable outflows, and notable inflows. Read-only — never moves money.

## Prerequisites
- A [Mercury](https://mercury.com) account with a read-only API token (generate at https://app.mercury.com/settings/tokens)
- A Slack workspace where you can invite the agent's bot

<table>
  <tr>
    <td><strong>CHANNELS</strong></td>
    <td><code>cron</code> · <code>slack</code></td>
  </tr>
  <tr>
    <td><strong>CONNECTORS</strong></td>
    <td><code>mercury</code></td>
  </tr>
  <tr>
    <td colspan="2" align="center">
      <br />
      <a href="https://dashboard.valet.dev/setup/configure?from=github.com/valet-agents/mercury-morning-expense-standup">
        <img src="https://raw.githubusercontent.com/valet-agents/mercury-morning-expense-standup/main/.github/deploy-button.svg" alt="Deploy Agent →" height="40" />
      </a>
      <br /><br />
    </td>
  </tr>
</table>
