# Project Configuration

## Setup

Before making any changes, always run the project setup to ensure your environment is compatible:

```
bun run setup
```

This validates runtime versions, checks tool availability, and syncs CI telemetry. It must complete successfully before any code changes.

## Fix Guidelines

- Type errors: Fix the type annotations directly in the source files
- Lint errors: Run `bun run format-lint`
- Always run `bun run setup` first to ensure environment compatibility
