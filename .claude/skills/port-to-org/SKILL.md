---
name: port-to-org
description: Migrate the wild-west monorepo from a personal GitHub account to a GitHub org so the Sheriff push ruleset (10 MB file size block) can be enabled. Personal repos cannot host push rulesets.
---

# Port wild-west to a GitHub org and enable the Sheriff

The Sheriff push ruleset blocks any push containing files larger than
10 MB. **It requires an org-owned repository.** GitHub.com personal repos
cannot host push rulesets at all - the API returns
`"Source only org-owned repos can have push rules"`. This is true on
github.com; on GitHub Enterprise Server the same restriction does not
apply (push rulesets work on personal repos in GHES 3.11+, subject to
enterprise policy).

This skill walks through the migration.

## Steps

1. **Identify or create the destination org.** Confirm with the user:
   - Existing org they admin? Use it.
   - New org? Create at `https://github.com/account/organizations/new`
     (free tier is fine for push rulesets).

2. **Transfer the repo.** Web UI is most reliable:
   `https://github.com/<current-owner>/<repo>/settings` -> Danger Zone ->
   Transfer ownership.

   Or via API:
   ```bash
   gh api -X POST /repos/<current-owner>/<repo>/transfer \
     -f new_owner=<org-name>
   ```

3. **Verify the move:**
   ```bash
   gh api /repos/<org>/<repo> --jq '.owner.type'
   ```
   Should return `Organization`.

4. **Enable the Sheriff push ruleset:**
   ```bash
   cat <<EOF | gh api -X POST /repos/<org>/<repo>/rulesets --input -
   {
     "name": "Sheriff: block oversized files",
     "target": "push",
     "enforcement": "active",
     "rules": [
       {"type": "max_file_size", "parameters": {"max_file_size": 10}}
     ]
   }
   EOF
   ```

5. **Confirm:**
   ```bash
   gh api /repos/<org>/<repo>/rulesets --jq '.[] | {name, target, enforcement}'
   ```
   Should list the new ruleset with `target: push`.

6. **Update README.md.** Remove the "Sheriff not active" section so the
   docs reflect that the Sheriff is now live. The CHARTER's note about
   sandbox-only deferral should also be removed.

7. **Update CODEOWNERS if the org uses teams.** Personal-handle entries
   (`@DuaneNielsen`) may need to become org/team handles
   (`@<org>/<team-slug>`) depending on conventions. Ask the user.

## Notes

- The branch ruleset on `main` and both workflow files (Deputy, Reaper)
  port unchanged - same JSON, same YAML.
- After transfer, the old `<user>/<repo>` URL redirects to the new
  `<org>/<repo>` location. Existing clones keep working; users should run
  `git remote set-url origin <new-url>` at their leisure.
- If the destination is GitHub Enterprise Server (not github.com), the
  ruleset POST works identically against the GHES API host. Adjust
  `GH_HOST` accordingly when calling `gh`.
