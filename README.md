# CI/CD Template (GitHub Actions)

A reusable, project-agnostic pipeline. One set of workflow files lives in each
repo; each project customizes behavior through a thin caller + a `with:` block.
Versioning is automatic from commit messages (Conventional Commits → SemVer tags).

## Architecture

```
push/PR to main
   ├─ ci-<stack>.yml  ──► ci-reusable.yml  (lint → build → test, per stack)
   └─ commitlint.yml  ──► rejects non-conventional commits

push to main (merge)
   └─ release-please.yml  ──► opens "Release PR"
        merge Release PR
          └─ cuts tag vX.Y.Z + GitHub Release + updates CHANGELOG

push of tag v*
   └─ deploy-staging.yml  ──► deploy-reusable.yml  (environment: staging)
```

Key rules:
- **CI never deploys.** Deploy only fires on a version tag.
- **One build, promote the artifact** through environments (don't rebuild for prod).
- **main is always green** — branch protection blocks merges on red CI.

## Per-project setup (5 minutes)

1. Copy `.github/` into the repo. Keep `ci-reusable.yml`, `deploy-reusable.yml`,
   `release-please.yml`, `commitlint.yml` as-is.
2. Copy the matching caller (`ci-node.yml` / `ci-python.yml` / `ci-go.yml` /
   `ci-ios.yml`) and edit its `with:` block (commands, versions, scheme names).
3. Copy `release-please-config.json` + `.release-please-manifest.json`.
   Set `release-type` to match the stack: `node`, `python`, `go`, or a
   [release-please release-type](https://github.com/googleapis/release-please).
4. Add `commitlint` dev dependency and `commitlint.config.js`.
5. Add `./scripts/deploy.sh` (or edit the deploy-command in `deploy-staging.yml`).

## Branch protection (run once per repo, needs `gh` + auth)

```bash
gh api repos/:owner/:repo/branches/main/protection \
  -X PUT \
  -f required_status_checks.strict=true \
  -f "required_status_checks.contexts[]=call-ci" \
  -f "required_status_checks.contexts[]=commitlint" \
  -f enforce_admins=true \
  -f required_pull_request_reviews.count=1 \
  -f dismiss_stale_reviews=true \
  -f required_linear_history=true \
  -f restrictions=null
```

Replace `:owner/:repo` with the actual slug. The status-check context `call-ci`
is the job name in the caller workflow — rename it there if you change it.

## Secrets (repo Settings → Secrets → Actions)

- `DEPLOY_TOKEN` — used by deploy-reusable.yml (your staging/prod credentials).
- `CODECOV_TOKEN` — optional, for coverage upload in ci-reusable.yml.

## Conventional Commits (drives versioning)

```
feat: add retry to uploader        → MINOR bump
fix: null deref in parser          → PATCH bump
feat!: drop node 18 support        → MAJOR bump (! = breaking)
```

Release-please opens a Release PR; merging it cuts `vX.Y.Z` and a GitHub Release.
The tag then triggers the staging deploy automatically.

## Adding a prod deploy

Copy `deploy-staging.yml` → `deploy-prod.yml`, point `environment: prod`, gate it
with a manual approval environment or a `workflow_dispatch` trigger. Reuse
`deploy-reusable.yml` so prod and staging share one deploy implementation.
