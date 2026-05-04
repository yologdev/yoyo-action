# yoyo-action

GitHub Action to run [yoyo](https://github.com/yologdev) coding agents on your repo.

**Free for open source. [Sponsor](https://github.com/sponsors/yologdev) for private repos.**

## Quick Start

1. Add `ANTHROPIC_API_KEY` as a repository secret
2. Copy workflow templates from `templates/` to your `.github/workflows/`
3. Create `.yoyo/yoyo.toml` in your repo
4. Label an issue `ready` — yoyo builds it

## Agents

| Agent | Workflow | Trigger | Default |
|-------|----------|---------|---------|
| **PM** | `yoyo-pm.yml` | Daily 6am UTC | Enabled |
| **Build** | `yoyo-build.yml` | `ready` label + 4h cron | Enabled |
| **Review** | `yoyo-review.yml` | PR opened by yoyo | Enabled |
| **Office Hour** | `yoyo-office-hour.yml` | Daily 7am UTC + issue open | Opt-in |
| **Research** | `yoyo-research.yml` | Sundays 9am UTC | Opt-in |

## Default Flow

```
PM suggests issues (labeled "triage")
  → You approve → label "ready"
  → Build implements → creates PR
  → Review checks → approves → auto-merges
```

## Configuration

Create `.yoyo/yoyo.toml`:

```toml
[commands]
build = "pnpm build"
test = "pnpm test"
lint = "pnpm lint"

[protected]
paths = [".github/", ".yoyo/yoyo.toml"]

[agents.pm]
enabled = true

[agents.build]
enabled = true

[agents.review]
enabled = true

[agents.office-hour]
enabled = false         # set to true to enable auto-triage

[agents.research]
enabled = false         # set to true to enable weekly scans
```

## Pricing

| Tier | Price | Access |
|------|-------|--------|
| Open Source | Free | Public repos, all agents |
| Sponsor | TBD | Private repos |

## How It Works

1. Workflow triggers → calls `yologdev/yoyo-action@v1`
2. Action checks sponsor status (private repos only)
3. Pulls `ghcr.io/yologdev/yoyo-harness:latest` Docker image
4. Runs the requested agent with your API key
5. Agent reads your code, implements changes, creates PRs

Your `ANTHROPIC_API_KEY` is the only secret needed. You pay Anthropic directly for API usage.

## License

MIT
