```
                          Welcome to the

           __      ___ _    _ __      __        _
           \ \    / (_) |__| |\ \    / /__  ___| |_
            \ \/\/ /| | / _` | \ \/\/ / -_)(_-<|  _|
             \_/\_/ |_|_\__,_|  \_/\_/\___|/__/ \__|

                                              __u_____ gL/
                                          .-ayZ@@@@@@@@@@_
                                       _aa@@@@@@@@@@@@@@@@g
                                     -L_yyg@@@@@@@@@@@@@@@@$_
                                      C~$@@@@@@@@@@@@@@@@@@@@g
                ______yg$@@@$ggyyyyyyyg@@@@@@@@@@@@@F``~~?R@@@@
       _   __a@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@~       `F~'
  __2E@gg@@@@@@@@`y@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@`
f~FTZE$@@@@@@M@~  $@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@$
    `~~TR=F~-F    $@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@$
                 _@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@L
              yyg@@@@@@@@@@~   ~~FPRRRRRRRPP@@@@@@@$gy_
             a@@PF~`y@@@@~                   `~@@@@F4@@$
            a@~    @@@P~                        ~@@L  `~@g_
          a@P      ~@$                           `R@L    4@g_
       a@@PF        4@_                            ~@g     R@$
       ~`            9@_                             @@y
                      ~@@$                            `9@g
```

> OWNERS govern. The Deputy enforces.
> Push fat files — the Sheriff turns you around.
> Sit idle too long — the Reaper takes the land.

# wild-west

A collaborative monorepo that maximizes developer autonomy while keeping the
repo itself healthy. Four moving parts keep the peace:

📜 **The Land Deeds** (`projects/*/OWNERS`) — You claim your project folder,
you decide who works there, and you sign your treaty (`Ironclad Guarantee`,
`Move Fast and Break`, or `At Your Own Risk`).

🤠 **The Deputy** (`.github/workflows/project-guard.yml`) — Ensures outsiders
don't mess with your land without your written approval.

🚔 **The Sheriff** (GitHub push ruleset) — Stands at the border and stops
anyone trying to smuggle 10 MB+ binaries into the repo, keeping the town
history clean.

💀 **The Grim Reaper** (`.github/workflows/midnight-reaper.yml`) — Comes out
in the dead of night to burn down abandoned ghost towns (stale
`At Your Own Risk` projects) and delete stale branches.

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
