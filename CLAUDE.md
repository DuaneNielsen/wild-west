# Claude project notes - wild-west monorepo

This is a collaborative monorepo with **minimal central review**. Read
[`README.md`](README.md) for the overview and [`docs/CHARTER.md`](docs/CHARTER.md)
for the full rules before doing anything else here.

## Quick reference

- **Projects live under `projects/<name>/`.** Nothing else gets added at the root.
- **Every project has an `OWNERS` file** declaring its owners and stability contract.
- **Allowed top-level paths** (enforced by `.github/workflows/project-guard.yml`):
  - `projects/`, `.github/`, `.claude/`, `docs/`
  - `README.md`, `CLAUDE.md`

Anything else at the repo root will be rejected by the Deputy on PR.

## When the user asks you to...

- **Start a new project** -> invoke the `new-project` skill in `.claude/skills/`.
- **Change a project's stability contract** -> invoke the `change-contract` skill.
- **Port this repo to a GitHub org** (so the Sheriff push ruleset can be enabled)
  -> invoke the `port-to-org` skill.
- **Add or remove an owner from an existing project** -> just edit
  `projects/<name>/OWNERS` directly. No skill needed.

## Things to never do

- Add a file at the repo root that isn't in the allowed list above.
- Push files larger than 10 MB (the Sheriff push ruleset rejects them when active;
  see README's "Sheriff not active" section for the current sandbox status).
- Modify `.github/workflows/`, `CODEOWNERS`, or `CLAUDE.md` without going through
  admin review - these are perimeter changes and require the listed code owner.
- Delete a project folder unless the Midnight Reaper is the one opening the PR.

## Where things live

| Path | What |
|---|---|
| `.github/workflows/project-guard.yml` | The Deputy (PR gate) |
| `.github/workflows/midnight-reaper.yml` | The Grim Reaper (nightly cleanup) |
| `.github/CODEOWNERS` | Repo-perimeter ownership |
| `projects/example-app/OWNERS` | Canonical OWNERS file template |
| `docs/CHARTER.md` | Full design + rationale |
| `.claude/skills/` | Repo-specific Claude skills |
