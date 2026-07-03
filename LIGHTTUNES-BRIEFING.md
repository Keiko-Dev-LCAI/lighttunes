# LightTunes — App Briefing & Changelog

Live: https://lighttunes.win · Local folder: `~/Desktop/lighttunes-app/` · Repo: `Keiko-Dev-LCAI/lighttunes` (GitHub Pages, branch `main`)

Backend/relay: shared OrcaVault relay `orcavault-production.up.railway.app` (Railway project **vivacious-empathy**), code in `~/Desktop/orcavault-app/relay-server/server.py`.

Thumbnails/covers are stored in this repo under `thumbs/` (served from GitHub raw) and written by the relay using its Railway `GITHUB_TOKEN`.

---

## Changelog

### Session 147 — 2026-07-02
- **Per-song cover button restored.** Owner-only **🖼️ Cover** button on each song card — only the song's uploader sees it. Lets the uploader set/replace that song's cover art.
  - Frontend: commit `659d738` (`index.html` — button + `pickSongThumb` / `handleSongThumbFile` / `saveSongThumbToRelay`).
  - Relay: commit `953a595` in `orcavault-app` — new `POST /api/lighttunes/set-song-thumbnail`, verifies the caller signature and confirms on-chain that the wallet is the song's uploader (via `SongCreated` event) before saving `thumbs/v1_<songId>.jpg`.
  - Pattern mirrored from LightTube's working `set-thumbnail` flow (apps reference each other).
- **"Cover failed: 401" fixed.** The relay's Railway `GITHUB_TOKEN` env var (on **vivacious-empathy**) was the OLD revoked PAT, so GitHub rejected the thumbnail upload. Fix: set `GITHUB_TOKEN` to the current valid `keiko-pc` PAT (kept in `~/Desktop/orcapod-app/GITHUB-TOKEN.txt`) and redeploy. This also silently restored album covers and upload-time thumbnails, which use the same token.
- **Now-playing cover size fixed.** The player artwork was rendering as a giant full-width square, pushing title/artist/badges/action buttons below the fold. Capped it: `#player-art-wrap` → `max-width:min(440px,46vh)`, centered, rounded. Commit `386d14a`. Everything now sits in view without scrolling.
