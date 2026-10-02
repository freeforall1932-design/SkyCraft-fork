# Port — our divergence from upstream SkyCraft

Everything we build on top of SkyCraft lives in this folder: planning, analysis,
and eventually code. Upstream is [chasmlol/SkyCraft](https://github.com/chasmlol/SkyCraft);
this repo is a fork of a fork, working on branch `arena/01a0fb2f-skycraft-fork`.

Fork point: **`bfcaf17`** (SkyCraft 0.1.2) — see `notes/` for analyses pinned to it.

## Why this folder is sync-proof

Git merges only conflict where **both sides changed the same files**. Upstream has
no `port/` folder and never will (it's our namespace), so everything in here
survives every upstream merge untouched, with zero conflicts. No copying of the
fork, no re-diffing by hand — `git merge` does the work.

## Rules of engagement

1. **All our work stays under `port/`.** Notes in `port/notes/`, plans in
   `port/PLAN.md`, our code in `port/code/` (when we get there).
2. **Don't edit upstream files** (`skse/`, `fabric/`, `protocol/`, `tools/`,
   `docs/`, root README) for as long as upstream syncs matter. Every upstream
   file we touch is a future merge conflict. If we genuinely must change one,
   record it in `port/TOUCHPOINTS.md` so the next sync knows what to re-check.
3. **Pin every analysis to a commit hash.** Notes here reference upstream code
   as `path @ <hash>`. Even if upstream later deletes or rewrites that file,
   `git show <hash>:<path>` still shows exactly what we analyzed.

## Syncing from upstream

```bash
git fetch upstream
git log --oneline HEAD..upstream/main   # what's new?
git merge upstream/main                 # port/ survives automatically
```

After a sync, check which pinned analyses went stale:

```bash
git diff --stat <pinned-hash>..upstream/main -- skse/ fabric/ protocol/
```

If a path shows up there (or `git show` says it's gone), re-read it and refresh
the note with a new pinned hash. That's the whole maintenance cost.

We use `merge`, not `rebase`: our commits keep their history and nothing gets
rewritten. If the day comes that our code edits upstream files heavily (a true
hard fork), we stop syncing and re-evaluate — that's a decision, not an accident.

## Where our code will go

- `port/code/` — self-contained projects that don't need SkyCraft's build system
  (a host plugin for another engine has its own SDK and CMake anyway).
- A vendored copy of the shared-memory protocol header, with the upstream commit
  it was taken from recorded next to it, goes in `port/code/vendor/` when needed.
- Only if we ever *modify* SkyCraft itself (e.g. de-Skyrim-ifying the protocol)
  do we edit upstream files — and then rule 2 applies: log it in TOUCHPOINTS.md.
