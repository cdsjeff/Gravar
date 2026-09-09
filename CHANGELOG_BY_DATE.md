# Gravar Changelog by Date

> **Dating note:** Dates below are based on the Gravar development record available in this project. Where an exact release date was not recorded, the revision remains under **Date not recorded** rather than being assigned an invented date.

## 2026-09-09

### v0.6.185 — Force PT Timecodes Explicit Only

- Corrected parent-timecode inheritance so **Force PT timecodes** must be checked before a child tier receives its parent tier's Start/Stop/Duration.
- Applies to both italic-Speaker child tiers and `((...))` transcript child tiers.
- With Force PT off, child tiers preserve their own timing, including waveform-selected intervals.
- Main transcript and transcript pop-out now use the same rule.
- Updated CA Help/tooltips and current-version metadata.

### v0.6.184 — Child-Tier Mode Gating

- Repaired automatic tier conversion of parenthesized transcript rows when **CA → Child tiers** is off.
- Parentheses markers now have tier meaning only while Child tiers mode is explicitly enabled.
- Waveform-created rows containing single or double parentheses retain their own unique Start/Stop/Duration when Child tiers is off.
- Applied the same rule to the main transcript and transcript pop-out.
- Updated CA/Transcript Help and current-version metadata.

## 2026-08-26

### v0.6.183 — Code Filter Initialization Repair

- Repaired the initialization error `this.filterLabels is not a function`.
- The v0.6.182 filter methods had been attached to `XSymbols` rather than `XTalkActionPalette`, even though the Code-pane UI invokes them through `XTalkActionPalette`.
- Moved all four filter methods to the correct Code palette object and removed the misplaced copies.
- Preserved Any/All matching, parent/child grouping, ±1 context, transcript pop-out parity, and saved filter state.
- Updated current-version metadata to v0.6.183.

### v0.6.182 — Code-Based Transcript Filter

- Added a compact Filter toggle to the right-side Code pane.
- Existing Code buttons become filter selectors while Filter mode is active.
- Added Any/All matching and optional ±1 parent-level context.
- Matching parent/child tier units remain intact, including when the code is on a child tier.
- Preserved original line numbers and synchronized filtering with the transcript pop-out.
- Filter state is view-only and saved in project JSON.

## 2026-08-18

### v0.6.181 — Collapsible Code Sections

- Made each Code-pane section independently collapsible/expandable by clicking its section header.
- Added disclosure triangles and project/Code-set persistence for collapsed state.
- Edit mode reveals all Code buttons while editing.
- Updated Code-pane Help and build metadata.

## August 10, 2026

### v0.6.178 — Waveform Calibration Import in Project / Import

- Added **Waveform calibration** to **Files → Import**.
- The command invokes the same calibration-recovery workflow available in the Waveform calibration pane.
- Imports only waveform alignment/calibration data from an older Gravar project JSON.
- Preserves the current transcript, media assignment, codes, notes, drawings, and other project settings.
- Retained the original **Wave → Waveform calibration → Import** control.

### v0.6.177 — Waveform Calibration Recovery Import

- Added an **Import** control to the **Waveform calibration** pane.
- Allows waveform alignment information to be recovered from an older Gravar project JSON after media must be reloaded manually.
- Imports the saved Absolute offset and all saved Point calibration values.
- Migrates older single drift-anchor calibration data into the current Point format.
- Supports older projects containing only `waveformOffsetSeconds` when no newer calibration fields exist.
- Does not replace the current transcript, media assignment, codes, notes, drawings, or other project settings.
- Resets the temporary waveform nudge after import so recovered offsets are not applied twice.

### v0.6.176 — Waveform Interval Timing Fix

- Repaired **Command–Option waveform selection** insertion so the new transcript row uses the onset and offset of the selected waveform interval.
- The selected onset and offset are frozen when the Insert dialog opens.
- **Play** previews that same frozen interval.
- **Insert** writes the selected onset to Start and selected offset to Stop.
- Removed dependence on the active or preceding transcript row for timing during insertion.
- Applied the repair to both the main waveform and the video pop-out waveform.

## August 5, 2026

### v0.6.175 — Waveform Point Undo

- Added a small **Undo** button immediately beside **Clear** in the Waveform calibration pane.
- Undo reverses only the most recent **Point** calibration command.
- Restores the previous set of calibration points.
- Restores the temporary waveform nudge used to create the undone point.
- The button remains disabled until a Point command has been made.
- Undo does not reverse **Absolute**, **Clear**, or unrelated waveform operations.

## July 28, 2026

### v0.6.172 — Electron Desktop Edition

- Added a repository-ready Electron desktop edition of Gravar.
- Added native Project-folder designation and direct filesystem access.
- Enabled direct project saves, autosaves, exports, FrameClips, V-Clips, and `Old_files` management.
- When no Project folder is active, the folder containing the first imported project or media file can become the Project folder.
- Added native folder and save dialogs.
- Media is attached immediately before optional recovery storage.
- Removed the Electron edition’s dependence on IndexedDB for loading large media files.
- Preserved **Command–M** for Merge rather than allowing Electron to use it for minimizing the application.
- Added a restricted preload bridge with Node integration disabled and context isolation enabled.
- Retained a standalone browser HTML edition.

## Date not recorded

### v0.6.174 — Waveform Selection Preview

- Added a **Play** button to the dialog opened after **Command–Option-dragging** across the waveform.
- Users can preview the selected interval before inserting a transcript row.
- Preview plays the selected interval once while keeping the dialog open.
- The dialog provides **Cancel**, **Play**, and **Insert**.
- Closing or canceling the dialog stops an active preview.
- Supported in both the main waveform and the video pop-out waveform.

### v0.6.173 — Split Transcript Line

- Added **Option–Return** to split a transcript line at the text cursor.
- Text before the cursor remains in the original row.
- Text after the cursor moves into a new row immediately below.
- Speaker, child-tier status, and Group membership carry into the new row.
- Start, End, and Duration remain blank in the new row.
- Codes and Line Notes remain with the original row.
- Works in both the main transcript and transcript pop-out.
- Supports **Command–Z Undo**.

### v0.6.171 — Final Idle Autosave and Project-Folder Recovery

- Added one final autosave after five minutes of inactivity before autosave pauses.
- Activity resumes the autosave schedule.
- Added a **Designate** workflow for Chrome/Edge Project-folder authorization.
- When folder authorization is missing or expired, **Save** prompts the user to re-designate the Project folder.
- Manual saves, autosaves, exports, and `Old_files` management use the designated folder.
- Autosave numbering scans the folder for the highest existing number so numbering does not restart on another computer or browser session.
- Excess autosaves are archived for each recognized Gravar autosave series.
- Safari continues to use browser recovery storage and manual Save As because it cannot provide a persistent writable folder handle.

### v0.6.170 — Safari Save Workflow

- Added a Safari-specific **Save As…** workflow.
- Changed **Folder** to **Save location** when persistent folder access is unavailable.
- Added status messaging indicating that Safari users must choose a location at each save.
- Preserved Chrome/Edge direct Project-folder behavior.
- Clarified that Safari cannot infer or retain the folder from which media was imported.

### v0.6.169 — Imported Speaker Label Normalization

- Standardized imported default speaker labels from `Speaker 1` to `Spkr 1`.
- Applied normalization to DOTE, TSV, appended project/TSV, and ELAN TSV imports.
- Preserved custom speaker names without modification.

### v0.6.168 — MacWhisper DOTE Import

- Added **Files → Import → DOTE**.
- Imports MacWhisper DOTE JSON directly into Gravar.
- Maps `startTime` to Start, `endTime` to End, `speakerDesignation` to Speaker, and `text` to Transcript.
- Calculates Duration automatically.
- Validates JSON and the top-level `lines` structure.
- Sorts imported rows chronologically.
- Refreshes the transcript, waveform, and transcript pop-out.
- Adds an Undo checkpoint, autosave integration, and import statistics.

### v0.6.166 — Compact Video Rollups

- Reorganized the Video pane into compact collapsible sections.
- Added rollups for **Gestures**, **Adjustments**, and **Tools & Pop-out**.
- Left-justified section labels and removed unnecessary explanatory rows while collapsed.
- Combined Video tools and pop-out controls into a compact shared panel.
- Preserved V1/V2 visibility, Stack, attached controls, Fr-Clip, and V-Clip functions.

### v0.6.165 — Independent V1/V2 Display Adjustments

- Added separate V1 and V2 adjustment banks.
- Added independent Brightness, Contrast, Color, Red, Green, and Blue controls.
- Added an option to link V1 and V2 adjustments.
- Added **Reset both**.
- Saved video display adjustments in the project.
- Included adjustment values in V-Clip specifications.

### v0.6.164 — Transcript Pop-out Count Repair

- Rewrote transcript pop-out word and utterance count functions.
- Removed problematic template-literal regular-expression escaping that could invalidate the pop-out script.
- Preserved live count updates and pause-marker exclusion behavior.

### v0.6.163 — Word and Utterance Counts

- Added **Words** and **Utts** fields between Duration and Speaker.
- Counts update live as transcript text changes.
- Parenthetical pause-marker content is excluded from both counts.
- Added visibility controls for the new fields alongside Start, Stop, Duration, and Speaker.
