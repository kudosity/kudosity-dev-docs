---
title: SMS GitHub Action
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity SMS GitHub Action sends an SMS from a GitHub Actions workflow. Drop it into any workflow step — on push, on schedule, on deploy success, or on failure — and it delivers a message via the Kudosity V2 SMS API.

The Action is published on the [GitHub Marketplace](https://github.com/marketplace/actions/kudosity-sms) as a single-purpose composite action — no Docker image, no JavaScript runtime.

## Features

With the SMS Action, you can:

* Send a single SMS as part of any GitHub Actions workflow
* Trigger sends from any workflow event (push, schedule, workflow\_dispatch, job failure, deploy success)
* Capture the Kudosity message ID and delivery status as step outputs for downstream verification
* Pin to the moving `@v1` tag for patch-level updates, or to a specific tag or commit SHA for exact reproducibility

## Installation

Reference the Action as a step in any workflow:

```yaml
- uses: kudosity/kudosity-sms-action@v1
  with:
    api-key: ${{ secrets.KUDOSITY_API_KEY }}
    to: '0491570156'
    from: ${{ secrets.KUDOSITY_SENDER }}
    message: "Deploy ${{ github.sha }} shipped"
```

Pin to `@v1` for patch-level updates, or to a specific tag or commit SHA (`@v1.0.1`, `@<sha>`) if you need exact reproducibility.

## Authentication

> ⚠️ Never inline the API key in workflow YAML
> The API key gives full access to your Kudosity account. Always store it as a repository, organisation, or environment secret and reference it via `${{ secrets.KUDOSITY_API_KEY }}`.

1. Get your API key from the Kudosity dashboard under **Developers → API Settings**.
2. In your GitHub repository, go to **Settings → Secrets and variables → Actions → New repository secret**. Name it `KUDOSITY_API_KEY`.
3. (Optional) Add `KUDOSITY_SENDER` as another secret for your sender ID, and reference it as `${{ secrets.KUDOSITY_SENDER }}` in the workflow `from:` input. This keeps sender IDs out of workflow YAML and lets you rotate them centrally.

## Example workflows

### Notify on deploy

```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - name: Deploy
        run: ./scripts/deploy.sh
      - name: Notify by SMS
        uses: kudosity/kudosity-sms-action@v1
        with:
          api-key: ${{ secrets.KUDOSITY_API_KEY }}
          to: ${{ secrets.ONCALL_PHONE }}
          from: ${{ secrets.KUDOSITY_SENDER }}
          message: "Deploy ${{ github.sha }} of ${{ github.repository }} shipped to prod."
```

### Alert on failure

```yaml
- name: Alert on failure
  if: failure()
  uses: kudosity/kudosity-sms-action@v1
  with:
    api-key: ${{ secrets.KUDOSITY_API_KEY }}
    to: ${{ secrets.ONCALL_PHONE }}
    from: ${{ secrets.KUDOSITY_SENDER }}
    message: "ALERT: ${{ github.workflow }} failed in ${{ github.repository }} (run ${{ github.run_id }})"
    message-ref: "alert-${{ github.run_id }}"
```

### Using the outputs

The Action exposes `message-id` (the Kudosity message ID, a UUID) and `status` (for example `queued` or `delivered`) as step outputs. Reference them from downstream steps:

```yaml
- name: Send SMS
  id: sms
  uses: kudosity/kudosity-sms-action@v1
  with:
    api-key: ${{ secrets.KUDOSITY_API_KEY }}
    to: ${{ secrets.ONCALL_PHONE }}
    from: ${{ secrets.KUDOSITY_SENDER }}
    message: 'Hello from CI'

- name: Log the message ID
  env:
    MSG_ID: ${{ steps.sms.outputs.message-id }}
  run: echo "Kudosity message ID was $MSG_ID"
```

## When to use

The SMS Action is best suited for:

* Notifying on-call staff when a build or critical scheduled job fails
* Sending a confirmation SMS on deploy success
* Scheduled reminders or heartbeats from cron-triggered workflows
* Alerting from any CI step that can express a failure condition

It is **not** the right fit for:

* Multi-recipient sends or list-based campaigns — use the Kudosity dashboard or the V1 API directly
* MMS or rich messaging — use the [GitHub Copilot Extension](/docs/copilot-extension), the other AI plugins, or the V2 MMS endpoint

## Runner requirements

The Action requires `bash` and `jq`. The default GitHub-hosted `ubuntu-latest` and `macos-latest` runners qualify out of the box. Windows runners are not supported.

For self-hosted or custom runners, install `jq` first:

```bash
# Debian/Ubuntu
sudo apt-get install -y jq

# macOS
brew install jq
```

## How it works

The Action runs entirely on the GitHub runner. It posts directly to `https://api.transmitmessage.com/v2/sms` with your API key in the `x-api-key` header. No third-party server is involved beyond the Kudosity API.

The API key is explicitly registered with the runner's log scrubber on each invocation, so it is redacted from any output the API might return.

There is no automatic retry on transient failures (5xx responses, network errors). If your use case needs retries, wrap the step with a retry wrapper such as [`nick-fields/retry@v3`](https://github.com/nick-fields/retry).
