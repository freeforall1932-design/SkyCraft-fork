# Side projects (2026-10-02)

## Shotgun King ammo/rework — patch-repo workflow

- New repo YES; dumping game files into it NO (creator IP + updates invalidate).
  Layout: .gitignore(game-dump,*.pck) / tools(recover.md, apply, repack.md) /
  modded/ (only changed files, mirroring res://) / notes/.
- Dev loop: gdre_tools --recover once → open in Godot editor → edit GDScript →
  run from source (F5) → godot --headless --export-pack for production.
  Cheat Engine only for quick recon on the stock game (find which var = shells);
  everything else is source editing, not memory patching.
- Feature pointers: ammo → search ammo/shell/reload in gun/player script;
  card picker → draft/offer logic (expand pool, manual keys, or pick_random);
  enemy picker → spawn/piece/floor logic, same pattern. Keep changes additive
  (debug autoload) to survive game updates.

## Extending the 2010 Rust Rewrite Mashup (IW4L)

- IW4L already reads MW3 (2011) and Black Ops zones into the same asset_iw4 IR
  (github.com/chasmlol/iw4L). MW3 weapons/maps as data = extend existing
  pipeline; full MW3 gameplay = same-engine-family adaptation (IW5), moderate.
- MW 2019 / MW3 2023 reboot: different engine generation (IW8), packed formats,
  online service games — not forkable, multi-year RE + legal heat. No.
- Ready-made classic CoD loadout expansion: AlterWare clients (alterware.dev)
  ship weapons/maps ported between classic CoDs; OpenAssetTools documents the
  on-disk formats (IW4L itself credits it).
- Rule: within engine generation = extension; across generations = recreate.
