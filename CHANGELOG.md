# Gravar Changelog

## v0.6.198 — 2026-11-08 — Default Symbols Persistence and Legacy Migration

- Fixed a regression in which loading an older project JSON could replace the expanded v0.6.195 default Symbols palette with that project's older built-in 5-row or 7-row palette.
- Added **`XSymbolDefaults`** as the single authority for Gravar's built-in Symbols baseline and legacy-default migration rules.
- Fresh Gravar sessions now initialize from the expanded shared default: **44 rows (37 symbols + 7 section headers)**.
- Exact historical Gravar built-in symbol palettes are automatically migrated to the expanded default when older projects are loaded.
- Genuinely customized project symbol palettes are preserved and continue to load project-specifically; the migration does not overwrite custom rows/descriptions/orderings.
- Moved Symbols project persistence into an **`XProjectState` adapter**, removing direct Symbols save/load ownership from `XFiles` and continuing the v0.6.197 modular architecture.
- Legacy top-level `symbolRows`, `activeSymbolRowId`, and `symbolEditMode` fields are still written for backward compatibility.
- The expanded shared symbol-set JSON remains included in the Current Resources package as `gravar-shared-symbols.json`.
- Validation passed for both embedded JavaScript blocks plus targeted fresh/default, legacy-5, legacy-7, expanded-default, missing-set, and custom-palette migration tests.

## v0.6.197 — 2026-11-08 — Modular Core Refactor and Efficiency Audit

- Completed the first deep architecture/efficiency pass aimed at long-term Gravar programmability rather than adding a user-facing feature.
- Added **`XBus`**, a lightweight cross-module message bus with immediate and coalesced messages.
- Added **`XAutosaveCoordinator`** so edit-triggered recovery autosaves are coalesced instead of repeatedly serializing/stringifying the whole project during typing bursts; periodic/final folder backups remain immediate.
- Added **`XMediaIdentity`** as the single authority for same-V1 identity matching used by project media recovery and calibration preservation.
- Added **`XCalibration`** as the single authority for waveform calibration mathematics, point normalization/interpolation, Absolute/Point/Undo/Clear transitions, legacy migration, and calibration persistence.
- Added **`XTranscriptHierarchy`** as the single authority for parent/child-tier classification, parentheses rules, italic-Speaker child tiers, parent association, and Force PT timing inheritance.
- Added **`XProjectState`**, an extensible project-state adapter registry that captures module-owned state once per save while continuing to write legacy top-level fields for backward compatibility.
- Drawing and Video Adjustments now register with `XProjectState` rather than monkey-patching `XFiles.payload()`.
- Added **`XCoreMessages`** for application-level message subscriptions and moved V1-attach, calibration-change, hierarchy-change, and project-load fan-out toward explicit messages.
- Removed the timing-dependent 250-ms post-project-load video-adjustment restore; Video Adjustments now react to the actual `project:loaded` completion message.
- Made the main transcript the authoritative parent/child-tier mutation engine; the transcript pop-out no longer independently retags rows.
- Coalesced transcript-pop-out synchronization to avoid redundant full-transcript messages during rapid edits.
- Added a **Modular core and message passing** section to Gravar Diagnostics, including service/adapter checks and runtime bus/autosave metrics.
- Reduced `XWaveform` substantially by moving calibration rules into `XCalibration`; the largest UI modules remain intentionally intact for later incremental refactoring rather than a high-risk rewrite.
- Added `ARCHITECTURE_AUDIT_v0.6.197.md` to the Current Resources package with findings, validation, compatibility notes, remaining hotspots, and recommended next stages.
- Preserved legacy project fields and existing UI/shortcut behavior; `moduleState` is additive.
- Validation passed for both embedded JavaScript blocks, modular-core regression tests, calibration/hierarchy/project-state/media-identity tests, autosave-coalescing tests, and static checks for removed serialization monkey-patches/timer coupling.
- A full automated browser/media end-to-end test was not available in the current container, so representative real-project/media field testing is still recommended before declaring v0.6.197 a long-term milestone.

## v0.6.196 — 2026-11-08 — Play Cursor Modifier Gating

- Corrected the main **Play** button cursor, which previously showed an up/down resize cursor during ordinary hover.
- The Play button now uses the normal pointer cursor unless the playback-speed modifier is actively held.
- On macOS, the up/down cursor appears only while **Command** is depressed; on non-Mac platforms the equivalent modifier is **Ctrl**.
- Added pointer-enter/move/leave and keydown/keyup synchronization so the cursor changes immediately while hovering.
- Added a window-blur reset so the special cursor cannot remain stuck after focus changes.
- Playback behavior and the existing Command/Ctrl-click playback-speed menu are unchanged.
- Updated current-version metadata and build consistency checks to v0.6.196.
- Both embedded JavaScript blocks passed syntax validation.

## v0.6.195 — 2026-11-08 — Expanded Shared Symbols Default Palette

- Replaced the prior small built-in Symbols palette with the newly supplied expanded Gravar symbol set.
- The default palette now contains **44 rows: 37 symbols and 7 section headers**.
- Preserved the supplied symbol IDs, descriptions, section titles, ordering, and active-symbol selection.
- Default sections are **Onset/Offset/Talk, Arrows, Bracket, Triangles & Dots, Square, Letters, and Lines**.
- Project-saved Symbols and manually imported symbol-set JSON continue to override the built-in defaults normally.
- Included the expanded shared symbol-set JSON in the Current Resources ZIP as `gravar-shared-symbols.json`.
- Updated current-version metadata, About/Help fallback text, and build consistency checks to v0.6.195.
- Both embedded JavaScript blocks passed `node --check`.

## v0.6.194 — 2026-11-08 — Shared Symbols Default Palette

- Loaded the supplied `gravar-shared-symbols.json` as Gravar's built-in default Symbols palette.
- Default Symbols now include **(.)** brief pause, **(0.0)** timed pause, **[** overlap onset, **]** overlap offset, **=** latching, **↑**, and **↓**.
- Preserved the supplied symbol IDs, descriptions, and active-symbol selection.
- Project-saved Symbols and manually imported symbol-set JSON continue to override the built-in defaults normally.
- Included `gravar-shared-symbols.json` in the Current Resources ZIP for reuse and reference.
- Updated current-version metadata, About/Help fallback text, and build consistency checks to v0.6.194.
- Both embedded JavaScript blocks passed `node --check`.

## v0.6.193 — 2026-11-08 — Project Files Order and Persistent Project Name

- Kept the established horizontal **Files → Project / Import / Export / Diag / Auto** tab layout.
- Reordered **Files → Project** so the visible sections are **Project**, **Codes & Symbols**, then **Project files**.
- **Project files** is now a separate collapsed rollup below **Codes & Symbols** rather than nested inside the Project controls.
- Moved **Project name** out of the Project rollup and made it a single persistent row at the bottom of the Files pane.
- **Project name** now remains visible when **Project, Import, Export, Diag, or Auto** is selected.
- Removed the former nested Project-name placement so only one Project-name input remains in the DOM.
- Preserved the existing Project-files path/status panel, Last-3/Full path toggle, and clickable pathname viewer.
- Updated About, Help, build metadata, version consistency checks, and release descriptions to v0.6.193.
- Verified one `projectName` ID, no duplicate static IDs, correct Project-section order, and valid syntax for both embedded JavaScript blocks.

## v0.6.192 — 2026-11-08 — Project Submenu Rollups

- Kept the established horizontal **Files → Project / Import / Export / Diag / Auto** tab layout from v0.6.191.
- Inside **Files → Project**, added two independent collapsed rollups: **Project** and **Codes & Symbols**.
- **Project** contains Load, Save, Close, Folder, Reconnect, Setup, the nested **Project files** status/path rollup, and **Project name**.
- **Project name** remains the final item inside the Project rollup, below the other Project controls.
- **Codes & Symbols** contains only the reusable Code-set and Symbol-set Import/Export controls.
- Both inner rollups start collapsed and can be opened or closed independently by click, Enter, or Space.
- Preserved the nested **Project files** rollup and its Last-3/Full clickable pathname viewer.
- Updated current-version metadata, About/Help text, and release description to v0.6.192.
- Both embedded JavaScript blocks passed `node --check`; no new duplicate static IDs were introduced.

## v0.6.191 — 2026-11-08 — Restore Files Tabs and Project Name Order

- Reverted the Files pane from the v0.6.190 accordion experiment back to the established v0.6.189 tabbed submenu layout.
- Restored **Project, Import, Export, Diag, and Auto** as the prior horizontal submenu tabs, with Project selected by default.
- Moved **Project name** inside the Project submenu and placed it **below all other Project controls**, including Project files and Codes & Symbols.
- Preserved the v0.6.189 collapsed **Project files** status/path rollup and clickable Last-3/Full pathname viewer.
- Updated current-version metadata and release descriptions to v0.6.191.
- Both embedded JavaScript blocks passed `node --check`.

## v0.6.190 — 2026-11-08 — Collapsible Files Submenus

- Converted **Files → Project, Import, Export, Diagnostics, and Autosave** from an always-open tab arrangement into compact vertical rollups.
- All five Files submenus now start collapsed; selecting a header opens only that section, and selecting the open header again rolls it closed.
- **Autosave** is fixed as the final Files submenu, below Project, Import, Export, and Diagnostics.
- Renamed the compact labels **Diag** and **Auto** to the explicit **Diagnostics** and **Autosave** headers.
- Moved the Project-name field into the Project rollup so it no longer sits outside the selected submenu.
- Preserved the nested **Project files** rollup inside Project; it remains collapsed by default.
- Updated Files Help, About/current-version text, XBuild metadata, and version consistency checks to v0.6.190.
- Both top-level embedded JavaScript blocks passed `node --check`.

## v0.6.189 — 2026-11-08 — Project Files Rollup and Clickable Path Viewer

- **Project files** under **Files → Project** is now rolled up by default and takes no status-panel space until its header is selected.
- Clicking the **Project files** header expands or collapses the file-location/status panel with a disclosure triangle.
- Replaced the prior path selector with an explicit toggle button that always shows the active mode: **Path: Last 3** or **Path: Full**.
- Clicking any Project JSON, Transcript, V1, V2, or Audio pathname now opens a full-width wrapped **Selected path** viewer below the list so long locations can actually be read and selected.
- The selected-path viewer follows the active Last-3/Full mode immediately.
- When the browser exposes only a filename or a path with three or fewer levels, the mode still changes visibly even though the displayed pathname may be identical; browser-hidden macOS absolute paths are not invented.
- Added keyboard access for opening the Project-files rollup and selecting a pathname.
- Updated Files help, static title/About/Help fallback text, XBuild metadata, and the version consistency fallback to v0.6.189.
- Both embedded JavaScript blocks passed syntax validation; Last-3 versus Full formatting was also checked with a long sample path.

## v0.6.188 — 2026-11-08 — Project File Locations and Load Status

- Added a compact **Project files** status block to **Files → Project**.
- Lists **Project JSON, Transcript, V1, V2, and Audio** with explicit status: **✓ Loaded/Saved**, **± Reference**, **✕ Missing/Failed**, or **— None**.
- Added a selectable path display: **Last 3 levels** or **Full**.
- Full displays the fullest location Gravar can actually know. Ordinary web file pickers may hide the macOS `/Users/...` absolute path; Gravar then shows a Project-folder-relative path or filename and identifies that limitation in the tooltip.
- Project-folder restores/reconnects update the media rows automatically, including marking expected media **Missing** when it is not found in the reconnected folder.
- Manual DOTE/TSV/ELAN transcript imports and V1/V2/audio loads update their corresponding file-status rows on loading, success, or failure.
- The selected path-display mode and known file-location metadata are saved in project JSON.
- Loading a different project rebuilds the status table so stale file locations from the prior project are not retained.
- Older project JSON remains compatible; Gravar reconstructs reference paths from saved filenames and Project-folder information where possible.
- Updated Files help and all current-version metadata to v0.6.188.

## v0.6.187 — 2026-10-22 — Project Folder Reconnect and Media Recovery

- Added a **Reconnect** control to **Files → Project** that is hidden during normal operation and appears only when a loaded project remembers a Project folder that Gravar cannot currently access.
- Chromium browsers can now store the selected Project-folder handle in IndexedDB. On a later project load, Gravar checks the remembered handle without triggering a permission prompt automatically.
- If the remembered folder is still authorized, Gravar reconnects it automatically. If permission is in the browser's `prompt` state, the Reconnect button lets the user renew access with an explicit click.
- If no usable remembered handle is available, Reconnect opens the folder picker so the user can choose the saved Project folder again.
- After reconnection, Gravar automatically searches the approved folder for the saved **V1**, **V2**, and external-audio filenames and restores any that are present.
- Project-folder media restoration preserves the waveform Absolute offset and Point calibration data already loaded from the project JSON.
- A Project folder from a previously loaded project is no longer searched while loading a different project.
- The remembered Project-folder name now remains in subsequently saved JSON even while folder permission is temporarily unavailable.
- Updated Files help and all current-version metadata to v0.6.187.

## v0.6.186 — 2026-10-22 — Project Media Calibration Persistence

- Diagnosed a project-reload failure path in which transcript and saved waveform calibration loaded correctly, but a missing/unavailable V1 video required manual reattachment.
- Manual **Load video** now preserves the project’s existing waveform offset and calibration points when the selected V1 matches the video identity saved by the project.
- Newer projects use the saved browser media key (filename, byte size, and modification metadata) for a stronger same-video check; older projects fall back to the saved filename.
- Loading a genuinely different V1 continues to clear waveform calibration, preventing old alignment from being carried onto unrelated media.
- Project-load status now explicitly tells the user that manually reattaching the same V1 preserves saved calibration.
- Updated in-app version/build metadata and Files help.

## v0.6.185 — Force PT Timecodes Explicit Only

- Deep diagnostic traced parent-timecode inheritance through the main child-tier tagging path, the v0.6.184 parentheses wrapper, and the transcript pop-out's independent retagging path.
- Repaired the condition that previously copied parent Start/Stop/Duration when a row first became a child tier even while **Force PT timecodes** was unchecked.
- **Force PT timecodes** is now the sole trigger for parent-to-child timing replacement.
- The rule applies equally to italic-Speaker child tiers and child tiers identified by transcript text beginning with double parentheses `((`.
- With Force PT unchecked, a child tier keeps its own Start, Stop, and Duration, including waveform-selected timecodes.
- With Force PT checked, parent Start/Stop/Duration is copied and re-enforced through normal retagging.
- Applied identical semantics to the main transcript and transcript pop-out.
- Updated CA controls, tooltips, Help, release metadata, and current-version description to v0.6.185.

## v0.6.184 — Child-Tier Mode Gating

- Repaired automatic parent/child tier tagging for parenthesized transcript text when **CA → Child tiers** is turned off.
- Diagnostic: the transcript-parentheses tier marker introduced in v0.6.125 classified `((...))` as a child tier independently of the Child tiers checkbox. Retagging then inherited the preceding parent row's timing and overwrote the waveform-selected Start/Stop values.
- Parentheses-based tier interpretation is now gated by the **Child tiers** setting in both the main transcript and transcript pop-out.
- With Child tiers off, transcript text beginning with single or double parentheses is ordinary transcript text and remains a full transcript row with its own Start, Stop, and Duration.
- With Child tiers on, the existing CA behavior is preserved: `((` can identify a child tier, a single `(` can identify a parent tier, and italic Speaker rows can identify child tiers.
- Updated CA/Transcript Help and all current-version metadata to v0.6.184.

## v0.6.183 — Code Filter Initialization Repair

- Repaired the startup failure `this.filterLabels is not a function` introduced in v0.6.182.
- Diagnostic: the new transcript-filter methods were accidentally attached to `XSymbols`, while Code-pane initialization and event handlers call them on `XTalkActionPalette`.
- Moved `filterLabels`, `toggleFilterCode`, `toggleFilterMode`, and `clearFilter` to `XTalkActionPalette`, which owns the Code-pane filter UI.
- Removed the misplaced duplicate methods from `XSymbols`.
- Preserved the v0.6.182 Code-filter behavior: Any/All matching, parent/child tier grouping, optional ±1 context, pop-out parity, and project persistence.
- Updated current-version metadata to v0.6.183.

## v0.6.182 — Code-Based Transcript Filter

- Added a compact **Filter** button beside the Code edit toggle in the right-side Code pane.
- The filter controls take no space while inactive.
- While Filter mode is active, existing Code buttons select or deselect filter criteria instead of applying codes to transcript rows.
- Matching is available as **Any** selected code or **All** selected codes.
- Parent and child tiers are treated as one transcript unit: if either a parent or one of its child tiers matches, the full parent/child unit is shown.
- Added optional **±1** context to include one preceding and one following parent-level transcript unit, including all of those units’ child tiers.
- Overlapping match/context ranges merge automatically.
- Original transcript line numbering is preserved in filtered views.
- Filtered viewing is non-destructive and does not modify, delete, or reorder transcript rows.
- Added main transcript and transcript pop-out filtering parity.
- Saved filter state in project JSON.

## v0.6.181 — Collapsible Code Sections

- Made every Code-pane section header independently clickable in normal coding mode.
- Clicking a section header hides or shows only the Code buttons belonging to that section, through the next section header.
- Added a disclosure triangle (`▼` expanded / `▶` collapsed) to each section header.
- Edit mode temporarily reveals all Code buttons so hidden sections remain fully editable.
- Saved each section's collapsed/expanded state in project JSON and reusable Code-set JSON.
- Updated Code-pane Help and current-version metadata.

## v0.6.180 — Current Version Metadata

- Centralized the current Gravar version and release description in the application code.
- Updated the browser title, About information, Help information, saved-project metadata, and V-Clip metadata to identify Gravar v0.6.180 consistently.
- Updated the source header to describe the current release rather than an obsolete v0.5.2 maintenance build.
- Current resource ZIP now includes an identical deployment copy of the standalone app named `index.html` for GitHub Pages and other static web hosting.

## v0.6.179 — Native .dote File Recognition

- Repaired MacWhisper DOTE import for files using the native `.dote` filename extension.
- The DOTE file chooser now accepts `.dote` as well as `.json`.
- Also accepts common JSON/text MIME types used by browsers for `.dote` files.
- The DOTE parser itself was already compatible: the supplied `.dote` file is valid JSON with a top-level `lines` array and 48 transcript lines.
- No transcript-field mapping behavior was changed.

## v0.6.178 — Waveform Calibration Import in Project / Import

- Added **Waveform calibration** to **Files → Import**.
- Uses the same calibration-recovery workflow as the Import control in the Waveform calibration pane.
- Imports only waveform alignment/calibration data from an older Gravar project JSON.
- Preserves the current transcript, media assignment, codes, notes, drawings, and other project settings.

## v0.6.177 — Waveform Calibration Recovery Import

- Added an **Import** control to the Waveform calibration pane.
- Recovers saved Absolute offset and Point calibration values from an older Gravar project JSON.
- Migrates older drift-anchor and legacy offset data when necessary.
- Leaves current transcript/media/project content intact.
- Resets the temporary waveform nudge after calibration import.

## v0.6.176 — Waveform Interval Timing Fix

- Repaired Command–Option waveform selection insertion.
- Start and Stop now use the selected waveform onset and offset rather than timing from the preceding transcript row.
- The selected interval is frozen when the dialog opens and is used by both Play and Insert.
- Applied to main and video pop-out waveforms.

## v0.6.175 — Waveform Point Undo

- Added a small **Undo** button beside **Clear** in the Waveform calibration pane.
- Undo reverses only the most recent Point calibration command.
- Restores the prior calibration-point set and the temporary waveform nudge used for that point.

## v0.6.174 — Waveform Selection Preview

- Added a **Play** button to the Command–Option waveform selection dialog.
- Preview plays the selected interval once while keeping the dialog open.
- Supported in both main and video pop-out waveforms.

## v0.6.173 — Split Transcript Line

- Added **Option–Return** to split a transcript line at the text cursor.
- Carries speaker, child-tier status, and Group membership to the new row.
- Leaves Start, End, and Duration blank in the new row.
- Works in main transcript and transcript pop-out and supports Command–Z Undo.

## v0.6.172 — Electron Desktop Edition

- Added a repository-ready Electron desktop edition with native project-folder and filesystem access.
- Added direct project saves, autosaves, exports, FrameClips, V-Clips, and `Old_files` management.
- Preserved Command–M for Merge and retained the standalone browser HTML edition.

## v0.6.171 — Final Idle Autosave and Project-Folder Recovery

- Added one final autosave after five minutes of inactivity before autosave pauses.
- Added Project-folder re-designation and folder-aware autosave numbering/archive handling.
- Preserved Safari browser-recovery/manual-Save-As behavior.

## v0.6.170 — Safari Save Workflow

- Added Safari-specific Save As behavior and save-location status messaging.
- Preserved direct Project-folder behavior in browsers that support persistent folder handles.

## v0.6.169 — Imported Speaker Label Normalization

- Standardized imported default speaker labels from `Speaker 1` to `Spkr 1` across supported transcript imports.
- Preserved custom speaker names.

## v0.6.168 — MacWhisper DOTE Import

- Added **Files → Import → DOTE** for direct MacWhisper DOTE JSON import.
- Maps timing, speaker, and transcript fields, calculates duration, validates input, sorts rows, and refreshes transcript/waveform views.

## v0.6.166 — Compact Video Rollups

- Reorganized the Video pane into compact collapsible Gestures, Adjustments, and Tools & Pop-out sections.

## v0.6.165 — Independent V1/V2 Display Adjustments

- Added separate V1/V2 Brightness, Contrast, Color, Red, Green, and Blue controls, optional linking, Reset both, project persistence, and V-Clip export values.

## v0.6.164 — Transcript Pop-out Count Repair

- Repaired transcript pop-out word/utterance count code and preserved live count updates with pause-marker exclusion.

## v0.6.163 — Word and Utterance Counts

- Added live Words and Utts fields with pause-marker exclusion and visibility controls.
