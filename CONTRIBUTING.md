# Contributing

## General workflow

1. Work in a focused branch.
2. Keep changes small enough to review.
3. Add or update tests for behavioural changes.
4. Run the repository's validation checks before opening a pull request.
5. Describe the operational impact and any migration/deployment requirements in the pull request.

## Python baseline

Python repositories should use:

- `uv` for dependency and environment management;
- `ruff` in strict configuration, including formatter checks;
- `mypy` in strict mode (or `ty` in strict mode when a repository explicitly adopts it);
- `pytest` for tests.

## Repository conventions

- Never commit secrets or customer data.
- Prefer explicit configuration through environment variables.
- Use Podman and OCI-compatible `Containerfile`/Compose definitions for reproducible services.
- Keep automation observable: failures should be logged and actionable.
- Customer-facing actions should remain reviewable unless explicitly designed and approved for unattended execution.

## Organization-wide source of truth

For organization members with access, the **single canonical product/value roadmap** is [`portfolio/product/roadmap.md`](https://github.com/jasperdelahaije/portfolio/blob/main/product/roadmap.md). The [portfolio delivery guide](https://github.com/jasperdelahaije/portfolio/blob/main/delivery/README.md) is navigation, not a parallel Issue/status database. The private [`ai-control/AGENTS.md`](https://github.com/jasperdelahaije/ai-control/blob/main/AGENTS.md) defines agent governance.

Technical implementation Issues, source code, tests, PRs and release evidence belong in the responsible component repository, **not** in `.github` or an additional central feature backlog. Historical [`.github#1`](https://github.com/jasperdelahaije/.github/issues/1) is closed and remains an archive; [`portfolio#1`](https://github.com/jasperdelahaije/portfolio/issues/1) documents the completed handoff. Existing [`business-ops#20`](https://github.com/jasperdelahaije/business-ops/issues/20) is the original Phase 6 epic, not an alternate organization-wide roadmap.

## CI trust boundary

**All GitHub Actions jobs must be self-hosted.** Trusted `push main` CI may use the approved `jd-ci` runner; untrusted `pull_request` / non-main validation must use the separately isolated self-hosted `jd-pr` group and `jd-pr-isolated` label. Privileged enrollment requires the separate `jd-admin` group. Do not fall back to GitHub-hosted or schedule untrusted code on persistent, production-adjacent runners; never give PRs deployment/SSH credentials. If isolated runner capacity is absent, checks remain blocked rather than bypassed. [Infrastructure P1 #83](https://github.com/jasperdelahaije/infrastructure/issues/83) tracks the **unresolved** runner/credential isolation. Require appropriate human approval for production deployment, secret changes and PR merge.

## Mandatory self-hosted policy for new repositories

Every new repository must start from the organization's **Python CI
(self-hosted only)** workflow template in `jasperdelahaije/.github/workflow-templates`,
or use an equivalent audited workflow. Client repositories created by
`project-bootstrap` should get the same trust-separated workflow as seeded
source. GitHub runner-group availability **does not automatically change a
workflow's `runs-on` selector**; repositories without any Actions workflows
need no runner, and a future custom workflow must follow this policy.

- Unreviewed `pull_request` and non-main source: `group: jd-pr` with
  `labels: jd-pr-isolated`. Never select persistent trusted runners.
- Reviewed `push` to protected `main`, trusted read-only CI and admin
  operations: `jd-ci` in `default` group (or a more restricted dedicated
  privileged group for privileged jobs).
- Rootless OCI builds, package publication and approved releases: `jd-builder`,
  **only** on reviewed `main` or authorized release events; PR jobs must not
  receive package write, production secrets, SSH keys or runner-admin tokens.
- Do not use `ubuntu-latest`, `windows-latest`, `macos-latest`, or a
  generic `self-hosted` label, or put GitHub-hosted selectors behind a silent
  fallback. For workflows that require unsupported operating systems or
  architectures, block execution until an appropriately isolated self-hosted
  runner exists.
- The `jd-pr` runner group is intentionally organization-wide
  (`visibility=all`, `allows_public_repositories=true`), also for future
  public repositories. That grants **eligibility**, not automatic routing.
  Rootless Podman still shares the production VPS kernel, especially risky
  for public fork PRs. Check every new repository's event triggers, permissions,
  network policy, runner labels and ephemeral cleanup before accepting CI.

**Organization-wide enforceable stop for GitHub-hosted jobs:** GitHub
provides **Organization Settings → Actions → General → Standard hosted
runners → Disable for all repositories → Save**. The owner must enable and
verify this setting; it has not been changed or read through the current
GitHub connector. Disabling hosted runners prevents a later arbitrary
`ubuntu-latest` workflow from running on GitHub-hosted capacity, but does
**not** rewrite that workflow's `runs-on`; incompatible jobs fail until
corrected. Keep the central template, generated seed and per-repository
workflow review alongside this organization setting. This policy does **not**
claim all current branches are merged or future custom workflows automatically
have working self-hosted routing. See
[GitHub's organization Actions guidance](https://docs.github.com/en/organizations/managing-organization-settings/disabling-or-limiting-github-actions-for-your-organization).
