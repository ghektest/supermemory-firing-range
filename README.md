# Supermemory Firing Range

Simulates the workflow_run chain attack found in supermemoryai/supermemory.

## Attack Chain

```
Fork PR (attacker) 
    → CI workflow runs on `pull_request` (fork code, no secrets)
    → CI FAILS (intentionally or due to attacker's bad code)
    → workflow_run fires `auto-fix-ci.yml` in PRIVILEGED context
    → Checks out attacker's branch (no fork guard!)
    → `bun install` runs attacker's package.json lifecycle scripts
    → Claude Code runs with Bash(*) in attacker's code directory
    → Secrets exfiltrated: CLAUDE_CODE_OAUTH_TOKEN, GITHUB_TOKEN (write)
```

## What's Missing (Intentionally)

The auto-fix workflow has **zero guards**:
- No `head_repository.full_name == github.repository` check
- No `branches:` filter on the workflow_run trigger  
- No actor/author_association check

## Two Attack Vectors

1. **Lifecycle scripts**: Attacker modifies `package.json` to add malicious `preinstall`/`postinstall` scripts. `bun install` executes them with full secret access.

2. **AI prompt injection**: Attacker adds `.claude/` config or `AGENTS.md` in their fork. Claude Code reads these and follows attacker instructions with `Bash(*)` access.

## Secrets Required

- `CLAUDE_CODE_OAUTH_TOKEN` - Claude Code API token (set in repo settings)

## How to Test

1. Fork this repo from another GitHub account
2. Modify `package.json` to add a malicious preinstall script  
3. Open a PR to this repo
4. CI runs and fails → workflow_run fires → attacker code executes in privileged context
