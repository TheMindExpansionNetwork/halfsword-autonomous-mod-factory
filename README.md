# Half-Sword Autonomous Mod Factory

> **The goal: Turn Grok (and eventually other agents) into a reliable factory that can research, design, generate assets, build, and publish complete Half-Sword mod packs on GitHub with minimal human intervention.**

This repository is the core of an **autonomous mod creation system** for *Half-Sword* (Unreal Engine 5.4.4).

---

## Vision

Half-Sword modding today is powerful but manual and fragmented:
- FModel for discovery
- UE 5.4.4 project for cooking
- repak for packaging
- UE4SS for runtime Lua magic
- Manual GitHub/Nexus uploads

This project automates the **orchestration, code generation, creative asset work (textures via AI), packaging, documentation, and publishing** so that an AI can take a request like:

> "Make a dark fantasy weapon pack with new flail textures and a UE4SS slow-motion training command, then publish it."

...and deliver a ready-to-use, versioned GitHub release.

---

## Two Parts of the System

### 1. Mod Factory (this repo)
- Python CLI (`hs-mod`)
- UE4SS Lua generator (excellent, safe, research-backed code)
- Pack scaffolding + manifest system
- repak wrapper
- Full documentation for autonomous workflows
- GitHub publishing automation (via `gh` or MCP tools)

### 2. Raw Asset Workspace (companion setup)
Located at `Z:\M0DD1NG\Games\Half-Sword\Raw-pak-backup` (or your equivalent).

This contains:
- Your encrypted game `.pak` backups
- `repak` + AES key
- Selective unpack scripts
- `workspace/` folder for actual editing

See [Raw Asset Workspace Setup Guide](#raw-asset-workspace-setup) below.

---

## Quick Start (Mod Factory)

```bash
# Clone this repo
cd /mnt/z/M0DD1NG/autonomous-tools-create-mods-ue

# Run the CLI
./scripts/hs-mod --help

# Create your first pack
./scripts/hs-mod new my-first-mod --type ue4ss --desc "Test autonomous mod"

# Generate high-quality Lua (slow-mo, buffs, etc.)
./scripts/hs-mod generate-lua my-first-mod

# Build it
./scripts/hs-mod build my-first-mod
```

See `docs/pipeline.md` for the exact prompts and stages Grok follows when working autonomously.

---

## Ideas for First Autonomous Projects

Here are concrete, achievable first targets we can build with the current system (ranked by autonomy level):

### Tier 1 – Extremely High Autonomy (Lua only, no cooking)

| Idea | Description | Difficulty |
|------|-------------|------------|
| **Slow-Mo Training Pack** | Console commands: `GrokSlow 0.25`, `GrokFast`, `GrokNormal`, health/stamina buffs | Already prototyped |
| **God-Mode / Invuln Trainer** | Toggle invulnerability + infinite stamina + one-hit kill toggle | Very easy |
| **Custom Loadout Commands** | Console commands that give specific weapon sets | Medium |
| **Stat Randomizer** | "GrokRandomize" that shuffles speed, power, grab force on spawn | Fun |
| **Photo Mode Enhancer** | Time dilation + FOV + no HUD commands | Easy |

### Tier 2 – High Autonomy (Lua + AI Textures)

- Dark Fantasy / Cursed Weapon Skins (use `image_gen` for new albedo/normal maps)
- "Blood Knight" armor texture variants
- Glowing rune effects on existing weapons
- Historical weapon re-skins (federschwert, etc.)

### Tier 3 – Medium Autonomy (requires selective asset work)

- Simple mesh swaps (polearm head replacements)
- New throwable objects (via data table + existing meshes)
- Custom map lighting tweaks or small prop additions

### Tier 4 – Future / Research Heavy

- Full new weapons (requires Windows UE cook step + Blender)
- AI-generated 3D meshes (when tools improve)
- Complex behavior mods (new AI patterns via UE4SS hooks)

**My recommendation for our first real public releases:**

1. **"GrokTrainer"** – A polished collection of the best console commands (slow-mo, buffs, god mode, etc.)
2. **"Cursed Weapons"** texture pack + matching Lua spawner commands
3. **"Historical Arms Pack"** – Accurate re-skins using the raw asset workspace

---

## Raw Asset Workspace Setup (Important)

This is where you actually edit real game files.

**Location** (as configured for this project):
`Z:\M0DD1NG\Games\Half-Sword\Raw-pak-backup`

It contains:
- `original-paks/` — your encrypted backups (never edit)
- `tools/repak` + `aes-key.txt`
- `scripts/unpack-selective.sh` and `list-assets.sh`
- `extracted/` — reference copies
- `workspace/` — your active editing area

**Typical flow**:
1. Use `./scripts/list-assets.sh ... "Weapons/Flail"` to explore
2. `./scripts/unpack-selective.sh "HalfswordUE5/Content/Assets/Weapons/Flail"`
3. Copy the files you want to change into `workspace/my-pack/Content/...`
4. Edit textures or uassets
5. Bring the changes into a pack created by `hs-mod new` in the mod factory

Full guide is in the `README-workspace.md` inside the raw folder.

---

## Documentation

- `docs/research.md` — Engine version, key objects (`Willie_BP_C`, `WorldSettings`), community links
- `docs/pipeline.md` — The autonomous playbook + ready-to-paste prompts
- `docs/asset-workflows.md` — How to do textures, Lua, full asset replacement
- `docs/publishing.md` — GitHub release conventions + MCP usage
- `AGENTS.md` — Rules for future AI sessions working in this repo

---

## Contributing / Using as Grok

If you're an AI (or human) wanting to work on this:

1. Read `AGENTS.md`
2. Read `docs/pipeline.md`
3. Use `./scripts/hs-mod` for all pack operations
4. Never commit game assets

---

## Current Status (May 2026)

- Strong MVP: `new` → `generate-lua` (excellent code) → `build` → publish flow works end-to-end for UE4SS mods.
- Texture generation via `image_gen` is ready to be wired deeper.
- Raw asset unpacking + AES key support is operational.
- Publishing via GitHub MCP or `gh` CLI is documented.

We are actively turning this into a **true autonomous mod factory**.

---

## License & Disclaimer

This is a tooling and research project.  
You must own a legitimate copy of Half-Sword.  
Never redistribute original game files.

Let's build something cool.

---

**Ready to create the first public repo and start shipping autonomous mods?**

Just say the word and I'll create the GitHub repository right now with all the current code + excellent documentation.