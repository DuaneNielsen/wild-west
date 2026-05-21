---
name: change-contract
description: Update the stability contract in a project's OWNERS file in the wild-west monorepo. Warns the user about Midnight Reaper implications when downgrading to "At Your Own Risk".
---

# Change a project's stability contract

The `contract:` field in `projects/<name>/OWNERS` declares the stability
tier. It determines whether the Midnight Reaper will auto-delete the
project for inactivity, and signals to consumers how stable the project's
APIs are.

## Contract tiers and Reaper behavior

| Tier | Reaper deletes? |
|---|---|
| `At Your Own Risk` | Yes - after 180 days of zero commits |
| `Move Fast and Break` | No |
| `Ironclad Guarantee` | No |

## Steps

1. **Read the current `OWNERS` file** at `projects/<name>/OWNERS`. Note the
   current contract and confirm with the user what they want to change it
   to.

2. **If downgrading to `At Your Own Risk`,** warn the user explicitly:
   > "This project will be auto-deleted by the Midnight Reaper after 180
   > consecutive days of no commits. The auto-PR will tag every owner
   > listed in the OWNERS file. Confirm you want to proceed?"

   Wait for explicit confirmation before editing.

3. **Edit the `contract:` line.** Preserve the quotes and use the exact
   tier string. Example:
   ```yaml
   contract: "Ironclad Guarantee"
   ```

4. **Branch and open a PR.** Branch name like
   `chore/contract-<project>-to-<new-tier>`. The Deputy passes
   automatically if the user is in the project's OWNERS file. Otherwise it
   needs an APPROVED review from one of the listed owners.

## Notes

- Don't change owners in the same PR unless the user asks. Keep contract
  changes scoped.
- If the user wants to upgrade from `At Your Own Risk` -> `Move Fast and
  Break` specifically to escape an imminent Reaper PR, the upgrade itself
  resets the 180-day clock (because the commit touches the OWNERS file).
  Mention this so they know the rescue worked.
