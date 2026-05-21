---
name: new-project
description: Scaffold a new project under projects/<name>/ in the wild-west monorepo. Creates the directory, an OWNERS file declaring owners and stability contract, and a starter README. Use when the user wants to start a new project in this repo.
---

# Start a new project in the wild-west monorepo

This repo requires every project to live under `projects/<name>/` with an
`OWNERS` file declaring stewardship and a stability contract. The Deputy
workflow enforces this on PR.

## Steps

1. **Pick a project name.** Lowercase, kebab-case (`billing-api`, not
   `BillingAPI`). Must not already exist under `projects/`. Confirm the
   name with the user before proceeding.

2. **Pick a stability contract.** Ask the user. Valid values:
   - `At Your Own Risk` - experimental sandbox. **Auto-reaped after 180
     days idle.** Tell the user this consequence explicitly.
   - `Move Fast and Break` - active development, no backward-compat
     promises. Default unless the user signals otherwise.
   - `Ironclad Guarantee` - production-grade. Breaking changes require
     advance notice to consumers.

3. **Identify the owners.** At least one GitHub handle (bare, no leading
   `@`). Default to the current user's handle. Ask before adding others.

4. **Create the files:**
   - `projects/<name>/OWNERS` - copy the format from
     `projects/example-app/OWNERS` exactly. The Deputy parses this with
     awk, so the structure matters (`owners:` list, `contract:` string in
     quotes, `description:` string).
   - `projects/<name>/README.md` - one-paragraph stub explaining what the
     project does.

5. **Branch and open a PR.** Branch name like `feat/init-<name>`. The
   Deputy will pass automatically because new projects use the homesteader
   rule (no prior OWNERS file means anyone can claim).

## OWNERS template reference

The canonical format is at `projects/example-app/OWNERS`. Read it before
scaffolding so the new file matches exactly. Key points:

- `owners:` is a YAML list, each entry is `  - <handle>` (no `@`).
- `contract:` value must be quoted and exactly one of the three valid
  strings above.
- Comments are fine but don't change field names.
