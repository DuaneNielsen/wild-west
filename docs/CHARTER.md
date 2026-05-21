# The Wild West Charter

This monorepo trades centralized review for distributed ownership. Anyone can
homestead a new project under `projects/`. Owners govern their own land. The
automation enforces a small set of repo-wide rules — and only those.

## The four mechanisms

### 1. Land Deeds (`OWNERS` files)

Every directory under `projects/` must contain an `OWNERS` file. The file
declares:

- **owners:** a list of GitHub handles who can self-merge changes to this
  project and whose APPROVED review unblocks PRs from non-owners.
- **contract:** one of three stability tiers (see below).
- **description:** a one-liner for the project.

See `projects/example-app/OWNERS` for the template.

### 2. The Deputy (PR gate)

`.github/workflows/project-guard.yml` runs on every PR and on every PR review
event. For each project a PR touches, it passes if either:

- the PR author is in that project's `OWNERS` file, **or**
- the project did not yet have an `OWNERS` file (homesteader rule — first
  claim wins), **or**
- at least one handle in the project's `OWNERS` file has submitted an
  APPROVED review on the PR.

Otherwise the check fails and blocks the merge until an owner approves.

PRs that touch files outside `projects/`, `.github/`, `docs/`, or `README.md`
are rejected outright — the perimeter is locked.

### 3. The Sheriff (push ruleset)

A repo-wide push ruleset blocks any push containing a file larger than 10 MB.
The rule is evaluated server-side before any ref updates, so oversized blobs
never enter history. The developer's local files are untouched — only the
push is refused. Standard remediation: rewrite the offending commit
(`git filter-repo`, `git reset`, `git rm --cached`) and re-push.

> **Status in this sandbox:** the Sheriff is not configured here because
> github.com personal repos cannot host push rulesets. The design works
> unchanged on any org-owned repo (free GitHub orgs included), and on
> GitHub Enterprise Server. See README's "Sheriff not active" section for
> the one-shot enablement command.

### 4. The Grim Reaper (nightly cleanup)

`.github/workflows/midnight-reaper.yml` runs at 00:00 UTC daily.

**Stale branches:** any remote branch that is not the default branch, not on
the protected list (`main`, `develop`, `staging`, `release/*`, `hotfix/*`,
`support/*`), has had zero commits in 30+ days, is not merged into the
default branch, and has no open PR — is deleted.

**Abandoned projects:** any project whose `OWNERS` file declares
`contract: "At Your Own Risk"` and has had zero commits in 180+ days is
deleted from the default branch via an auto-opened PR. The PR description
tags the original owners. The PR still has to be merged by a human; closing
it preserves the project.

## Stability contracts

| Tier | Promise | Reaped? |
|---|---|---|
| `At Your Own Risk` | None. May break or vanish at any time. | After 180 days idle. |
| `Move Fast and Break` | API will change. No backward-compat promises. | No. |
| `Ironclad Guarantee` | Production-grade. Backward-compatible. Breaking changes require advance notice. | No. |

The tier you pick determines how aggressively the Reaper treats your project
and what consumers can expect.

## What the charter does NOT do

- It does not enforce code style, test coverage, or build pass on the
  default branch. Owners pick their own tooling per project.
- It does not enforce review on the default branch beyond the Deputy.
  Owners can self-merge. If you don't trust yourself, add more owners.
- It does not gate CI on every project. The repo-wide rules are minimal.
  Each project's tooling lives under its own folder.

## Adding or changing the rules

The `.github/` directory is owned by repo admins (see `.github/CODEOWNERS`).
Changes require their approval — that's the one centralized chokepoint.
