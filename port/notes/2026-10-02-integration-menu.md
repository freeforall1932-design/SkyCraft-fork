# Integration menu & repo catalog (v2)

Researched: 2026-10-02 (two search sweeps) · Complements `2026-10-02-repo-map.md`
Levels: **offline pipeline** (convert + write saves) vs **live bridge** (games running).

## A. Game → Minecraft

| Tool | Live? | Role |
|---|---|---|
| Amulet-Core (Amulet-Team) | no | Python read/write MC saves — writer end |
| VoxelEarth (ryanhlewis) | streaming | Voxelize → place chunks around the moving player (Google 3D Tiles → MC); architectural reference for a live host exporter |
| binvox / Minecraftify | no | Mesh → voxels → block palette |
| FAWE / WorldEdit | in-server | Bulk block writes, async |
| Arnis / Terra 1-to-1 | no | Real-world cities/terrain → MC worlds |
| **Raspberry Jam Mod** (arpruss/raspberryjammod) | **yes** | Python API driving MC in real time (blocks, entities, chat); classic external-control endpoint |
| **WebDisplays + MCEF** (CinemaMod) | **yes** | Chromium rendered onto in-world block faces; proves MC can display arbitrary live external content (render-target pattern) |

## B. Minecraft → game / external

| Tool | Live? | Role |
|---|---|---|
| Mineways / jmc2obj / mc2obj (FalconNL) | no | MC world → OBJ/STL meshes |
| **MCprep** (Moo-Ack-Productions) | no | Blender addon: import MC worlds, swap texture packs, animate — the MC→DCC→game-engine asset pipeline |
| mineflayer / node-minecraft-protocol (PrismarineJS) | yes | Headless scriptable MC client (Node.js) |
| prismarine-viewer (PrismarineJS) | yes | Live world viewer in a browser — live MC → web rendering |
| **MCProtocolLib** (Steveice10 → GeyserMC fork) | yes | Java protocol-level headless MC client/server — MC without running the game |

## C. Live game ↔ game bridges (rarest category)

| Project | Mechanism |
|---|---|
| **SkyCraft** (this fork) | Shared memory, same machine, one renderer |
| **Geyser** (GeyserMC, ~5.6k★) | Network protocol translator: Bedrock clients ↔ Java servers. Biggest live cross-game bridge; study its handling of item/behavior mismatches between games |
| ViaVersion / ViaProxy | Version↔version protocol translation (incl. Bedrock via ViaBedrock) |
| **nestrischamps-emulator-connector** (Stabyourself) | Emulator Lua reads NES memory per frame → websocket upload. Template for "old game → external program" live link |
| BizHawk Lua / Dolphin-Lua-Core / dolphin-memory-engine | Per-frame scripted read/write of emulated game memory — any old console game as a host |
| VM Computers mod | VirtualBox PC inside MC (games-in-MC via full VM) |
| 2010 Rust Rewrite Mashup (chasmlol) | Clean-room engine rewrite path (no anti-cheat exposure) |

## D. Host-adapter foundations (build the missing side)

- **Unity:** BepInEx, MelonLoader (+ Harmony for runtime patching)
- **Unreal:** UE4SS
- **Bethesda:** SKSE/CommonLibSSE family (what SkyCraft uses), F4SE/CommonLibF4
- **Old/open engines:** Doom/Quake source ports — full engine ownership, nothing to hook
- **Emulators:** the Lua/memory stacks in §C

## E. Where to look for more

- GitHub topic pages: `geysermc`, `protocol-translator`, `minecraft-protocol`, `minecraft-proxy`, `minecraft-mod`, `voxel`
- Orgs: GeyserMC, PrismarineJS, PaperMC, EngineHub, Amulet-Team, CinemaMod
- awesome-minecraft (LiteDevelopers fork, maintained)
- awesome-game-hacking list — host-side memory/reversing tooling
- Modrinth / CurseForge for distributable mods
- TASVideos (BizHawk ecosystem), HN Algolia searches, YouTube reveals → follow to repos

## F. FPS / Godot addenda (2026-10-02)

- **Shotgun King (Godot, 2D)** — direct-file modding route: gdsdecomp full project
  recovery from .pck → edit decompiled GDScript (ammo system etc.) → repack.
  No runtime memory work needed; Steam verify/update reverts changes (back up pck).
  Official mod system exists but is finicky (load_mod registration, replace flag,
  version match) — direct file edit sidesteps the loader.
- **FPS-to-FPS weapon expansion** — two viable routes:
  1. Content port: extract + reimplement (GCFScape, Crowbar; AMX Mod X for
     GoldSrc servers, SourceMod for Source, open SDKs for Quake/UT).
  2. Same-engine mount: the Garry's Mod model (one engine family only).
  Live two-FPS bridges are architecturally awkward (both games want camera +
  input); feasible narrow form = external armory process syncing loadout state
  into per-game adapters. Never touch CS2/VAC; GoldSrc/Source-era on own
  servers is fine.

## Integration rule (avoids bloat + upstream conflicts)

External repos are **dependencies, not merges**: called as tools (pip, maven, binary,
server plugin) from port/code/ scripts. Their code never lands in this repo.
