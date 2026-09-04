---
name: Weekly Report Status
description: Generate a weekly activity report for commits, issues, and pull requests.
intent: Publish a concise weekly report issue for repository activity.
engine: copilot
on:
  schedule:
    - cron: "0 9 * * 1"
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
  cli-proxy: true
safe-outputs:
  mentions: false
  allowed-github-references: []
  max-bot-mentions: 0
  create-issue:
    title-prefix: "[weekly-report] "
    max: 1
---

# Weekly Report Status

## Task

Generate a concise activity report for the previous seven full days ending at workflow start (UTC) for this repository.

Collect and summarize:
- Commits merged or pushed during the report window, grouped by day or author when useful.
- Issues opened, closed, or updated during the report window.
- Pull requests opened, merged, closed, or updated during the report window.

Create one new issue with a title that starts with the configured `[weekly-report] ` prefix and includes the report window end date.

The issue body must:
- Use `###` headings for main sections.
- Include a short summary and counts for commits, issues, and pull requests.
- Include concise details with links for notable items when available.
- State clearly when no activity occurred in the previous seven days. If there were no commits, issues, or pull requests in the window, publish the issue anyway with that no-activity statement.
- Avoid unnecessary mentions and issue/PR backlinks except for links needed to identify reported items.

## Safe Outputs

- Use only the configured `create-issue` safe output to publish the report.
- Create at most one issue.
- If issue creation is not required because of a runtime error or missing access, use `noop` with a concise explanation.