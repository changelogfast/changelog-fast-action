# changelogfa.st Release Notes — GitHub Action

Generate a polished release note on every release and open it as a **pull request on `CHANGELOG.md`** for you to review and merge. Release notes that write themselves.

## Setup (one click, no API key)

1. **Install the [changelogfa.st GitHub App](https://github.com/apps/changelogfa-st/installations/new)** on the repo. That's the only setup — it provides the auth, the write access used to open the PR, and your free monthly quota.
2. Add the workflow below.

```yaml
# .github/workflows/release-notes.yml
name: Release Notes
on:
  release:
    types: [published]   # or: push -> tags: ['v*']

permissions:
  id-token: write        # required — keyless auth via GitHub OIDC

jobs:
  changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: changelogfast/changelog-fast-action@v1
        with:
          style: technical   # or: marketing
```

That's it. On each release the Action opens a `CHANGELOG.md` PR. **No API key, and no `contents`/`pull-requests` permissions needed in your workflow** — the changelogfa.st App opens the PR server-side; the workflow only proves its identity via OIDC (`id-token: write`).

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `style` | `technical` | Release note style (`technical`, `marketing`, …). |
| `api-url` | `https://changelogfa.st` | API base URL. Override only for self-hosting/testing. |

## Outputs

| Output | Description |
|--------|-------------|
| `pr-url` | URL of the opened `CHANGELOG.md` pull request. |

## How it works

The Action requests a short-lived **GitHub OIDC token** (no stored secret) that proves which repo it runs in. changelogfa.st verifies it, confirms the App is installed on that repo, generates the note from your release, and opens the PR. Re-runs for the same version reuse the same PR.

## Limits

The free tier covers a number of release notes per month per installation. Need more? **[Upgrade](https://changelogfa.st/pricing).**
