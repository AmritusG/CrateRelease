# MaCrate

A clip library for VJs and visual artists on macOS. Point it at your video folders and browse, search, tag, rate,
brief and measure your clips, then send them to Resolume Arena. No import step: MaCrate watches the folders and keeps
up as files change.

**Current version: 0.17.1** — [download MaCrate-0.17.1.dmg](https://github.com/AmritusG/CrateRelease/releases/download/v0.17.1/MaCrate-0.17.1.dmg)
([SHA-256](https://github.com/AmritusG/CrateRelease/releases/download/v0.17.1/MaCrate-0.17.1.dmg.sha256),
[release notes](https://github.com/AmritusG/CrateRelease/releases/tag/v0.17.1)).
Universal (Apple Silicon and Intel), macOS 14 or later, signed and notarized.

0.17.0 is MaCrate rebuilt in Rust on the cross-platform core it shares with Glance. It reads and writes the same
library as 0.16.4, so updating needs no migration and going back keeps everything.

---

## What MaCrate does

### Browse
- Add any number of folders or drives (**Add Folder…** in the sidebar). MaCrate indexes every video in them, makes
  thumbnails and shows them in a grid whose cell size and fields you choose (**View ▸ Settings**, `Cmd-J`).
- Browse by folder in the sidebar, or search and filter across the whole library.
- Press `Space` to preview a clip: it plays from the first frame and loops, with sound, frame by frame if you step.
- DXV (DXT1, DXT5, YCG6, YG10) and HAP (Hap1, Hap5, HapY, HapM, HapA) are first-class; H.264, HEVC, ProRes and the
  other formats macOS plays work too. Alpha clips show over a checkerboard, with Resolume's Alpha Type choice for DXV.

### Find
Full-text search over file names, tags, mood, genre and briefs, combined with filters for rating, tags, BPM, mood,
genre, energy, duplicates and the measured values (loops only, cycle length, cuts, flashes, motion, text and more).
Sort by name, date, duration, rating, BPM, energy, loop score, cycle or at random. Save a search and its sort as a
preset.

### Tag, rate and brief
Per clip: a 5-star rating, tags, BPM, mood, genre, energy (0–10) and a brief (a few keywords). Briefs can be written by Anthropic's Claude with your own API key (one clip, a folder, or the
current matches; a pre-flight shows the count and the estimated cost; identical copies share one request) or
imported from JSON or CSV (**File ▸ Import Briefs…**, `Cmd-Shift-I`). The key is kept in your Mac's Keychain.

### Stashes
Collect clips from anywhere in the library into named stashes, in your own order. A stash is saved in a
`.crate-stashes.json` file in the library folder it belongs to, so it travels with the drive.

### Measurements
MaCrate's scanner measures each clip: whether it loops and how well, its cycle length, cuts, flashes, flicker,
motion, how much of it is black, and whether it shows text. Filter and sort by any of them.

### Duplicates
Clips are identified by content, so copies across folders and drives are found. A badge marks them, the info panel
lists the other copies, and tags and briefs carry across them.

### Send to Arena
Right-click a clip (or a selection) ▸ **Send to Arena** to load it into a layer and column of a running Resolume
Arena, locally or over the network, optionally triggering it after it loads.

---

## Installation

1. Download `MaCrate-0.17.1.dmg` from the [latest release](https://github.com/AmritusG/CrateRelease/releases/latest).
2. (Optional) Verify it:
   ```bash
   shasum -a 256 MaCrate-0.17.1.dmg
   ```
   Compare with `MaCrate-0.17.1.dmg.sha256` in the release.
3. Open the DMG and drag **MaCrate** to **Applications**.

The DMG and the app are signed with an Apple Developer ID (Team `3A3L2C6DFB`) and notarized by Apple, so Gatekeeper
accepts them on first launch.

To go back to the Swift version, install [0.16.4](https://github.com/AmritusG/CrateRelease/releases/tag/v0.16.4)
over it; your library and settings stay as they are.

---

## Known issues

See the [open issues](https://github.com/AmritusG/CrateRelease/issues) and each release's notes. In 0.17.1 (and 0.17.0): dropping
a folder can crash MaCrate when the app it was dragged from quits right after the drop (a macOS bug; drag from Finder
or use **Add Folder…**).

---

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Cmd-,` | Settings window (API key, scan options) |
| `Cmd-J` | View settings (grid fields, cell size, indicators) |
| `Cmd-Shift-I` | Import briefs from JSON or CSV |
| `Cmd-Z` | Undo the last rename |
| `Space` | Preview the selected clip |
| `Cmd-Click` / `Shift-Click` | Add to the selection / select a range |

---

## Where your data lives

- **Sidecars:** each clip's metadata is also written beside it as `.<clip file name>.crate-meta.json`, so it moves
  with the clip and survives a rebuilt library:

  ```
  my-clip.mov
  .my-clip.mov.crate-meta.json
  ```

  ```json
  {
    "brief" : "vibrant geometric kaleidoscopic neon",
    "bpm" : 128,
    "clip_id" : "<SHA-256 of the first MiB>:<file size>",
    "created_at" : 801234567.0,
    "energy" : 7,
    "genre" : "techno",
    "mood" : "energetic",
    "rating" : 4,
    "tags" : ["geometric", "neon"],
    "updated_at" : 801234567.0,
    "version" : 1
  }
  ```
- **Library database and settings:** `~/Library/Application Support/com.amritus.crate/` and the preferences domain
  `com.amritus.crate`. Thumbnails are a cache in `~/Library/Caches/com.amritus.crate/`.
- **Stashes:** `.crate-stashes.json` in each library folder.

Everything stays on your Mac. The only network traffic is to Anthropic when you ask for a brief (your key, your
request) and to Resolume Arena when you send a clip.

---

## Credits

Built by Amrit Rosell, with Claude (Anthropic). MaCrate ▸ Acknowledgements lists every library it uses and its
licence.

## Source code

This repository is the binary distribution channel for MaCrate. The DMG is free to download and use.
