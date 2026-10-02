# Contributing

## General workflow

1. Work in a focused branch.
2. Keep changes small enough to review.
3. Add or update tests for behavioural changes.
4. Run the repository's validation checks before opening a pull request.
5. Describe the operational impact and any migration/deployment requirements in the pull request.

## Repository conventions

- Never commit secrets or customer data.
- Prefer explicit configuration through environment variables.
- Use Docker for reproducible service dependencies where practical.
- Keep automation observable: failures should be logged and actionable.
- Customer-facing actions should remain reviewable unless explicitly designed and approved for unattended execution.
