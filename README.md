# Crate

A native macOS clip library for VJs and visual artists. Browse, search, tag, and brief your video clips — built for live-performance workflows with Resolume Arena and similar tools.

---

## What Crate does

### Browse your video library

Point Crate at your video folders — single files, nested folders, or external drives. It indexes every video, generates thumbnails, and presents them in a fast scrollable grid with adjustable cell size. The thumbnail engine uses Metal-accelerated decoding so even large libraries on slow drives stay responsive.

You can browse by folder (sidebar navigation) or operate in library mode (search across the entire indexed collection regardless of folder).

### Find clips instantly

Full-text search across filenames, tags, and brief content. Filter by:

- **Rating** (1-5 stars)
- **Tags** (chip-based filters, combinable)
- **BPM** (with tap-tempo input)
- **Mood** (freeform field)
- **Genre** (freeform field)
- **Energy level** (slider field)
- **Duplicates** (show only clips that have identical-content duplicates)

Filter chips combine with text search — narrow to "all my high-energy clips matching 'neon' that have duplicates" in one query.

### Tag and rate

Per-clip metadata:

- **5-star rating**
- **Freeform tags** (with autocomplete from existing tags)
- **BPM** with tap-tempo
- **Mood** (single freeform string)
- **Genre** (single freeform string)
- **Energy level** (slider 0–100)
- **Brief** (freeform description — usually 3-7 keywords, see below)

All metadata is stored alongside the clip in `.crate-meta.json` sidecar files. Portable across machines, version-controllable, never locked into a proprietary database. If you delete Crate's library database, the sidecars are the source of truth — re-indexing rebuilds the database from disk.

### AI-generated briefs

Generate descriptive briefs for clips using Anthropic's Claude. Briefs are short keyword descriptions of the clip's visual content — for example: `vibrant geometric kaleidoscopic retro animated` or `dark biomechanical chrome metallic intense`.

Three ways to generate briefs:

1. **Single clip** — right-click any clip → "Generate Brief". Useful for one-off curation.
2. **Bulk: this folder** — toolbar button "Brief Folder (N, ~$X.XX)" briefs every un-briefed clip in the current folder. Pre-flight dialog shows count and cost estimate before committing.
3. **Bulk: current matches** — when in library mode (search/filter active), the toolbar button reads "Brief Matches (N, ~$X.XX)" and operates on the current filtered set.

**Duplicate-aware deduplication.** Crate hashes clip content. When the same clip exists in multiple locations (cross-drive copies, renamed duplicates), bulk-brief generates the brief once per unique content hash and fans the result out to every sibling row. A 1,000-clip library with 200 duplicates costs you 800 API calls, not 1,000.

**Bring your own API key.** Get an Anthropic API key at https://console.anthropic.com. Crate stores it in your Mac's Keychain — never synced to iCloud, never logged, never echoed to disk.

### Detect duplicates

Crate hashes file content using xxHash3-128 for fast cross-drive identification. When the same clip exists in multiple locations:

- A duplicate badge appears on the cell of every affected clip.
- The info panel shows "N duplicates elsewhere" with clickable paths.
- A filter chip "Has duplicates" surfaces only duplicated clips for cleanup workflows.
- A library-wide "View All Duplicates…" sheet (Cmd-J menu) shows every duplicate group at once.
- Tags and briefs cascade: tag one duplicate, all instances stay searchable; brief one, all siblings inherit the brief.

### Import existing briefs

If you've used external tools to generate briefs, or are migrating from another system, import them via **File > Import Briefs…** (Cmd-Shift-I).

Supported formats:

- **JSON, three shapes:**
  - Flat object: `{ "clip_id_hash": "brief text", ... }`
  - Array of objects: `[{ "clip_id": "...", "brief": "..." }, ...]`
  - Wrapped: `{ "briefs": [{...}, ...] }`
- **CSV** with a `brief` column header. Field aliases: `clip_id` / `clipID`, `current_path` / `path` / `file_path`, `brief`.

Matching strategy: clip_id (content hash) first, with fan-out to every row sharing the same content; path fallback if no clip_id match. Pre-flight dialog shows match counts and what will be filled vs. skipped vs. unmatched. Optional "Overwrite existing briefs" checkbox.

---

## What a user can expect

- **Native macOS app.** Real AppKit/SwiftUI, not Electron. Fast, properly themed, integrates with macOS conventions (Cmd-,, Cmd-J, drag-and-drop, NSOpenPanel, Quick Look, dark/light mode).
- **Local-first.** All your library data stays on your machine. The only outbound network traffic is API calls to Anthropic when you generate briefs — and only with your explicit consent and your own API key.
- **No subscription, no account, no telemetry.** Download, run, use. Your library is yours.
- **DXV2.2 / DXV3 / HAP-aware.** Crate is built knowing VJ workflows. DXV2.2, DXV3, and HAP clips (Resolume's preferred codecs) get proper handling, including high-performance Metal-based thumbnail decoding. Standard codecs (H.264, ProRes, HEVC, etc.) work too via AVFoundation.
- **Apple Silicon native.** Built for M1/M2/M3/M4 Macs. Intel Macs are not currently tested.
- **macOS 14+ required.** Built against modern SwiftUI; earlier macOS versions are not supported.

---

## Installation

1. Download `Crate-0.6.33.2.dmg` from the [latest release](https://github.com/AmritusG/CrateRelease/releases/latest).
2. (Optional) Verify the SHA-256 checksum:
   ```bash
   shasum -a 256 Crate-0.6.33.2.dmg
   ```
   Compare against the `Crate-0.6.33.2.dmg.sha256` file in the release.
3. Open the DMG.
4. Drag `Crate.app` to your `Applications` folder.
5. Eject the DMG.
6. Launch Crate from Applications.

The DMG is signed with an Apple Developer ID (Team `3A3L2C6DFB`) and notarized by Apple, so Gatekeeper accepts it without warnings on first launch.

---

## Setting up brief generation (optional)

If you want AI-generated briefs:

1. Open Crate Settings (**Cmd-,**).
2. In the "Anthropic API Key" section, click **Enter API Key…**.
3. Paste your Anthropic API key (get one at https://console.anthropic.com).
4. Right-click any clip and choose **Generate Brief**, or use the bulk-brief toolbar button when browsing a folder or in library mode.

**Cost:** roughly **$0.005 per clip** at current Claude pricing. A 1,000-clip library costs about $5 to fully brief. Duplicate-content clips share one API call, so the real cost is often lower than the raw clip count suggests.

---

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Cmd-,` | Open Settings window (backend + API key) |
| `Cmd-J` | Open quick-prefs popover (display, filters, duplicate tools) |
| `Cmd-Shift-I` | Import briefs from JSON or CSV |
| `Cmd-Z` | Undo rename (when a clip was just renamed) |
| `Space` | Quick Look the selected clip |
| `Cmd-Click` | Multi-select clips in the grid |
| `Shift-Click` | Range-select clips |

---

## Sidecar file format

Crate writes one `.crate-meta.json` file per clip, named after the clip with a dot prefix:

```
my-clip.mov
.my-clip.crate-meta.json
```

Sidecar contents:

```json
{
  "clip_id": "<xxhash3-128 of clip content>",
  "version": 1,
  "rating": 4,
  "tags": ["geometric", "neon", "high-energy"],
  "bpm": 128,
  "mood": "energetic",
  "genre": "techno",
  "energy": 78,
  "brief": "vibrant geometric kaleidoscopic neon",
  "updated_at": 1716800000
}
```

Sidecars are portable. Copy a clip to a new machine and its sidecar comes with it; Crate picks up the metadata on first index.

---

## Pre-release status

Crate is at version **0.6.33.2** — actively used by its developer but not yet considered 1.0-stable. Behaviors may change, file formats are stable but not frozen, and you may find sharp edges.

Please file issues at https://github.com/AmritusG/CrateRelease/issues.

---

## What's currently in the box

This release (0.6.33.2) includes:

- Library indexing with FSEvents-based live updates
- Multi-root library support (any number of folders/drives)
- Path-keyed schema (v8) — cross-root duplicates are first-class rather than collapsed
- Full-text search with FTS5
- Filter chips for rating / tags / BPM / mood / genre / energy / duplicates
- Single-clip brief generation with Claude
- Bulk-brief with folder scope or library-match scope, deduplicated by content hash
- Brief import from JSON (3 shapes) and CSV
- Duplicate detection with cell badges, info-panel disclosure, library-wide duplicate sheet
- In-grid navigation between duplicate siblings
- Settings window with Anthropic API key management
- Sidecar-based metadata storage (portable, version-controllable)
- DXV2.2 / DXV3 / HAP / standard codec support

---

## Credits

Built by Amrit Rosell with extensive collaboration with Claude (Anthropic).

---

## Source code

This repository (`CrateRelease`) is a **binary distribution channel** for Crate. The source code lives in a separate private repository and is not currently public.

The DMG distributed here is free to download and use.
