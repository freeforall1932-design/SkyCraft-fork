# Port plan — SkyCraft architecture on another game

Status: Phase 0 complete (session 1, 2026-10-02) · Next: Phase 1 — pick target game

## Notes index (session 1)

| Note | What's in it |
|---|---|
| `notes/2026-10-02-repo-map.md` | Component coupling, six-point host checklist, portability table |
| `notes/2026-10-02-integration-menu.md` | External repo catalog (A–F) + where to find more |
| `notes/2026-10-02-ps2-route.md` | Emulator-host route; God Hand/Black verdicts; pnach stability |
| `notes/2026-10-02-side-projects.md` | Shotgun King workflow + IW4L/MW3 extension facts |
| `notes/2026-10-02-shotgun-king-plan.md` | **TAKE-AWAY** — move to its own repo, then delete from here |

## Goal

Reuse SkyCraft's bridge pattern — one game as the visible renderer/world, another
as the hidden player/world simulator, talking over shared memory — with a
different host game (or as a reusable two-game bridge kit).

Core insight from the repo analysis (see `notes/2026-10-02-repo-map.md`):
the architecture and the Minecraft half are portable; the host-side plugin is a
per-engine rewrite. Feasibility of a host is decided by six capabilities:
collision export, player puppeting, camera override, input interception, frame
overlay, and NPC read/damage.

## Open decisions

- [ ] **Target game** — not chosen yet. Candidates discussed:
  - Bethesda ladder: Enderal (~free) → Fallout 4 (~50–60% carries, CommonLibF4) → Starfield (~20–30%) → Oblivion/Morrowind (~25–40%)
  - Unity host: best non-Bethesda case (BepInEx/MelonLoader gives all six capabilities)
  - Unreal host: possible via UE4SS, harder camera/overlay story
  - Anything online/anti-cheat: rejected (ban risk), unless clean-room rewritten like the author's own 2010 Rust Rewrite Mashup
- [ ] **Integration shape** once a target is picked:
  patch upstream files (conflicts forever) vs. separate host plugin that only
  shares the protocol header (clean, preferred).

## Phases

| # | Phase | Exit criteria |
|---|---|---|
| 0 | **Analysis** (now) | Repo map + portability table pinned in `notes/`; six-point checklist applied to candidate games |
| 1 | **Target selected** | Checklist verified against the actual game + its modding tools, not just docs |
| 2 | **Skeleton** | Host plugin skeleton in `port/code/`: maps shared memory, handshake, unit-scale config; moves a cube in the target game from Minecraft input |
| 3 | **Vertical slice** | Walk the target game's terrain with Minecraft physics (collision export + puppet + camera), à la SkyCraft Phase 1 |

## Upstream watching

Upstream moves fast (0.1.x, shipped Sept 2026). Sync per `README.md` and refresh
pinned notes when `git diff --stat` reports drift in the halves we care about.
