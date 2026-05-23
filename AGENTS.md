# AGENTS.md — Working in the Half-Sword Mod Factory

This file tells future Grok (or other agent) sessions exactly how to operate inside this repository.

**Primary goal**: Use the `hs-mod` pipeline to autonomously create, iterate on, build, and publish Half-Sword (UE 5.4.4) mod packs to GitHub as quickly and correctly as possible.

---

## Golden Rules

1. **Always start here** — Read `README.md`, `docs/research.md`, and `docs/pipeline.md` before doing any work.
2. **Use the CLI for everything** — Prefer `./scripts/hs-mod new ...`, `generate-lua`, `build`, etc. over manual folder creation.
3. **Never ship original game assets** — Only replacements, new Lua, generated textures, and documentation.
4. **"Work on" a pack** — Use `./scripts/hs-mod workon <slug>` (or `cd packs/<slug>`) for iterative development.
5. **Generate Lua first** — `generate-lua` produces excellent, research-backed UE4SS code (TimeDilation, Willie_BP_C, safe FindFirstOf patterns). Customize from there.
6. **Build before publishing** — Always run `build` and verify the `build/` folder contains usable artifacts.
7. **Document as you go** — Update the pack's own `README.md` with install steps and the commands you added.
8. **Verification is mandatory** — After major changes, run the verification steps in `docs/pipeline.md`.

---

## Typical Autonomous Session Flow

```bash
# 1. Research (if needed)
#    Use web_search + read docs/research.md

# 2. Scaffold
./scripts/hs-mod new my-cool-mod --type ue4ss,texture-swap --desc "..."

# 3. Enter the pack
./scripts/hs-mod workon my-cool-mod
cd packs/my-cool-mod

# 4. Generate excellent Lua
../scripts/hs-mod generate-lua my-cool-mod

# 5. (Optional) Add textures via image_gen tool + place in Content/ or assets-src/

# 6. Build
../scripts/hs-mod build my-cool-mod

# 7. Inspect
ls -R build/

# 8. Edit README.md inside the pack with final install instructions

# 9. Publish (when gh or MCP available)
../scripts/hs-mod publish my-cool-mod --gh-user <you>
```

---

## Key Commands Reference

- `hs-mod new <slug> ...`
- `hs-mod list`
- `hs-mod workon <slug>`
- `hs-mod generate-lua <slug>`
- `hs-mod build <slug>`
- `hs-mod publish <slug> ...` (currently advanced stub)

All commands live in `src/hsmod/cli.py` and delegate to `manifest.py`, `packer.py`, `generators/lua_generator.py`.

---

## Lua Generation Guidelines (from research)

Use the patterns proven in `lua_generator.py`:
- `FindFirstOf("WorldSettings")` + `TimeDilation`
- `FindFirstOf("Willie_BP_C")` + Health / speed / grab force etc.
- `IsValid()` checks
- `RegisterConsoleCommand("GrokSomething", fn)`
- `print("[Grok] ...")` feedback

Never use dangerous functions that were removed in newer builds (ExplodeHead, etc.) unless the user specifically asks for legacy behavior.

---

## Publishing Notes

See `docs/publishing.md`.
- Preferred: `gh` CLI (`gh auth login` first).
- Full autonomy: `search_tool` → `use_tool` on `grok_com_github` (create_repository, push_files, create_release).
- Every release must contain the built artifacts + excellent README.

---

## When You Need Windows / UE / FModel

The pipeline is intentionally split:
- **Linux / this workspace**: 90% of the work (Lua, orchestration, textures via image_gen, packaging, GitHub).
- **Windows dev box**: FModel discovery + one-time UE 5.4.4 cook for complex mesh/material changes.

The `asset-workflows.md` and `pipeline.md` describe the exact hand-off.

---

## Common Pitfalls to Avoid

- Forgetting to run `build` after editing Lua or adding assets.
- Using the wrong mount point when manually calling repak (use the packer).
- Naming the UE project anything other than `HalfSwordUE5`.
- Distributing base game files.

---

## Memory & Continuity

- Update `~/.grok/memory/autonomous-tools-create-mods-ue-c7d55070/MEMORY.md` with any new successful patterns, command sequences, or Half-Sword object discoveries.
- The hello-ue4ss example in `examples/` is the living reference implementation.

---

**Follow this file and the docs/ and you will reliably ship working, published Half-Sword mod packs autonomously.**

Welcome to the factory.