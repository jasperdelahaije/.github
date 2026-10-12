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
