# Guild-BMAD — Git Remote & Sync Convention

This repo is used from **two workspaces**. Both must sync through **one** repo (the work fork)
and receive updates from the Guild source **one-way only**. This keeps work isolated from the
personal account.

## Remotes (canonical setup)

| Remote | Repo | Direction | Purpose |
|---|---|---|---|
| `origin` | `AyrehlDavis/guild-bmad` (work fork) | **push + fetch** | Where all work lives. Both workspaces sync here. |
| `upstream` | `ayesel/guild-bmad` (Guild source) | **fetch ONLY** | Receive Guild updates. Push is disabled. |

> `ProjectAquariusOrg/guild-bmad` was removed — it is neither the source nor our fork.
> Do **not** re-add it.

## Rules
1. **Push only to `origin` (AyrehlDavis).** Never push work to `upstream` (personal).
   The `upstream` push URL is intentionally disabled, so an accidental `git push upstream`
   will fail — that's by design.
2. **Updates flow IN only.** Get new Guild versions with `fetch upstream` + `merge upstream/main`.
3. **Commit identity must be `Ayrehl Davis <ayrehl.davis@ariseenergy.com>`** in every workspace.
   Verify with `git config user.email`. (A prior commit leaked in from an un-configured machine
   as `ayrehldavis@…mynetworksettings.com` — don't repeat that.)

## Get Guild updates from the source
```bash
cd guild-bmad
git fetch upstream
git merge upstream/main          # one-way pull from ayesel
git push origin main             # save to the work fork
cd ../ariseenergy-com-nextjs && ./sync-guild.sh   # link any new commands
```

## Save / sync your own work
```bash
git push           # → AyrehlDavis (origin)
# other workspace:
git pull           # → gets it from AyrehlDavis
```

## First-time setup on a NEW workspace
```bash
git clone https://github.com/AyrehlDavis/guild-bmad.git
cd guild-bmad
git remote add upstream https://github.com/ayesel/guild-bmad.git
git remote set-url --push upstream DISABLED_no_push_to_personal   # block push to personal
git config user.name  "Ayrehl Davis"
git config user.email "ayrehl.davis@ariseenergy.com"
```
