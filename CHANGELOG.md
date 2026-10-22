# Gravar Changelog

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
