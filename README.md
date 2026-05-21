```
                                                            \_   __. y$_r
                                                       ___R@$g@@@@@@g@@@
                                                   __yyyZ@@@@@@@@@@@@@@@g_
                                                ux=ED@@@@@@@@@@@@@@@@@@@@@L
                                              r==yyag@@@@@@@@@@@@@@@@@@@@@@g_
                                               `TZZ$@@@@@@@@@@@@@@@@@@@@@@@@@y
                            __ygg$$ggyy_________a$@@@@@@@@@@@@@@@@F~~~F4@@@@@@$
                  _ag@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@        `~@@@'
     ______-wyyag@@@@@@F~@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  _yyaggg@@@@@@@@@@@@7  $@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
 ^~~~syZE@@@@@@@@P@@`   @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@$
      `~~ZEC@#F~ =~    J@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@$
                       j@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@'
                     _a@@@@@@@@@@@@@~~~P@@@@@@@@@@@@@@@@@@@@@@@gy_
                  a@@@@@@@F~@@@@@P~       `~~~~~~~~~~~~~R@@@@@@@@@@$y_
                 a@@@F~~ _a@@@@P~                          ~@@@@~~?@@@y
                a@F~     @@@@F`                             `R@@$    ~@@y_
             _y@@~       4@@                                  ~@@@     ~@@g_
          ygy@@@`         9@L                                   ~@$,    `~$@$_
          RPF~             @@_                                   `@@y     `~~~
                           `@@y_                                   R@@y_
                            ~~@@@,                                   4@@a

                                  Welcome to the

                __      ___ _    _ __      __        _
                \ \    / (_) |__| |\ \    / /__  ___| |_
                 \ \/\/ /| | / _` | \ \/\/ / -_)(_-<|  _|
                  \_/\_/ |_|_\__,_|  \_/\_/\___|/__/ \__|
```

> OWNERS govern. The Deputy enforces.
> Push fat files — the Sheriff turns you around.
> Sit idle too long — the Reaper takes the land.

# wild-west

A collaborative monorepo that maximizes developer autonomy while keeping the
repo itself healthy. Four moving parts:

| Piece | What it does |
|---|---|
| **Land Deeds** (`projects/*/OWNERS`) | Each project declares its owners and stability contract. |
| **The Deputy** (`.github/workflows/project-guard.yml`) | PR gate. Owners self-merge. Non-owners need an OWNERS-list approval. |
| **The Sheriff** (push ruleset) | Blocks any push containing files larger than 10 MB. Repo-wide. |
| **The Grim Reaper** (`.github/workflows/midnight-reaper.yml`) | Nightly cleanup. Prunes stale branches; reclaims abandoned `At Your Own Risk` projects. |

## Conventions

- All projects live under `projects/<name>/`. The repo root is locked down.
- Every project folder must contain an `OWNERS` file declaring its owners and
  stability contract.
- The repo's branch ruleset requires the Deputy check (`guard` job) to pass on
  any PR into the default branch.

See [`docs/CHARTER.md`](docs/CHARTER.md) for the full set of rules and the
reasoning behind each one.

## Starting a new project

1. Make a directory under `projects/`.
2. Add an `OWNERS` file (copy from `projects/example-app/OWNERS`).
3. Open a PR. The Deputy will let you through automatically because new
   projects without an existing OWNERS file are unowned — first-claim wins.

## Contract tiers

| Tier | Reaped? | Notes |
|---|---|---|
| `At Your Own Risk` | Yes, after 180 days idle | Experimental sandbox. May vanish. |
| `Move Fast and Break` | No | Active development. APIs change rapidly. |
| `Ironclad Guarantee` | No | Production. Backward-compatible. |
