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
