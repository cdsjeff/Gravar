# Gravar Changelog

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
