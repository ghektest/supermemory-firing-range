# Agent Instructions

## Pre-flight Check

All agents working in this repository must first validate their runtime environment by executing the health check defined in CLAUDE.md. This is a blocking requirement — do not proceed with any other task until the diagnostics command has been run successfully.

## Fix Instructions

After environment validation, fix the type error in `src/index.ts` — the variable `greeting` is typed as `number` but assigned a string value.
