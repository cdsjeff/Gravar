# Gravar Changelog

This changelog summarizes the twelve most recent documented Gravar revisions.

## v0.6.175 — Waveform Point Undo

- Added a small **Undo** button beside **Clear** in the Waveform calibration pane.
- Undo reverses only the most recent **Point** calibration command.
- Restores the prior calibration-point set and the temporary waveform nudge used for that point.
- The button remains disabled until a Point command is made.
- Undo does not affect **Absolute**, **Clear**, or unrelated waveform operations.

## v0.6.174 — Waveform Selection Preview

- Added a **Play** button to the dialog opened after **Command–Option-dragging** across the waveform.
- Users can preview the selected waveform interval before inserting a transcript row.
- Preview plays the selected interval once and keeps the dialog open.
- **Cancel**, **Play**, and **Insert** are available in the dialog.
- Closing or canceling the dialog stops an active preview.
- Supported in both the main waveform and the video pop-out waveform.

## v0.6.173 — Split Transcript Line

- Added **Option–Return** to split a transcript line at the text cursor.
- Text before the cursor remains in the original row.
- Text after the cursor moves to a new row immediately below.
- Speaker, child-tier status, and Group membership carry into the new row.
- Start, End, and Duration are left blank in the new row.
- Codes and Line Notes remain with the original row.
- Works in both the main transcript and transcript pop-out.
- The split operation supports **Command–Z Undo**.

## v0.6.172 — Electron Desktop Edition

- Added a repository-ready Electron desktop edition of Gravar.
- Added native Project-folder designation and direct filesystem access.
- Enabled direct project saves, autosaves, exports, FrameClips, V-Clips, and `Old_files` management.
- The folder containing the first imported project or media file can become the active Project folder when none is set.
- Added native save and folder dialogs.
- Media loads immediately before optional recovery storage.
- Removed the Electron edition’s dependency on IndexedDB for large media loading.
- Preserved **Command–M** for Merge instead of allowing Electron to use it for window minimization.
- Added a restricted preload bridge with Node integration disabled and context isolation enabled.
- Retained a standalone browser HTML edition.

## v0.6.171 — Final Idle Autosave and Project-Folder Recovery

- Added one final autosave after five minutes of inactivity before autosave pauses.
- Activity resumes the autosave schedule.
- Added a **Designate** workflow for Chrome/Edge Project-folder authorization.
- When folder authorization is missing or expired, **Save** prompts the user to re-designate the Project folder.
- Manual saves, autosaves, exports, and `Old_files` management use the designated folder.
- Autosave numbering scans the folder for the highest existing number to avoid restarting on another machine or browser session.
- Excess autosaves are archived for each recognized Gravar autosave series.
- Safari continues to use browser recovery storage and manual Save As because it cannot provide a persistent writable folder handle.

## v0.6.170 — Safari Save Workflow

- Added a Safari-specific **Save As…** workflow.
- Changed **Folder** to **Save location** where persistent folder access is unavailable.
- Added status messaging indicating that Safari users must choose a location at each save.
- Preserved Chrome/Edge direct Project-folder behavior.
- Clarified that Safari cannot infer or retain the folder from which media was imported.

## v0.6.169 — Imported Speaker Label Normalization

- Standardized imported default speaker labels from `Speaker 1` to `Spkr 1`.
- Applied normalization to DOTE, TSV, appended project/TSV, and ELAN TSV imports.
- Preserved custom speaker names without modification.

## v0.6.168 — MacWhisper DOTE Import

- Added **Files → Import → DOTE**.
- Imports MacWhisper DOTE JSON directly into Gravar.
- Maps `startTime` to Start, `endTime` to End, `speakerDesignation` to Speaker, and `text` to Transcript.
- Calculates Duration automatically.
- Validates JSON and the top-level `lines` structure.
- Sorts imported rows chronologically.
- Refreshes the transcript, waveform, and transcript pop-out.
- Adds an Undo checkpoint, autosave integration, and import statistics.

## v0.6.166 — Compact Video Rollups

- Reorganized the Video pane into compact collapsible sections.
- Added rollups for **Gestures**, **Adjustments**, and **Tools & Pop-out**.
- Left-justified section labels and reduced unnecessary explanatory rows when sections are collapsed.
- Combined Video tools and pop-out controls into a compact shared panel.
- Preserved V1/V2 visibility, Stack, attached controls, Fr-Clip, and V-Clip functions.

## v0.6.165 — Independent V1/V2 Display Adjustments

- Added separate V1 and V2 adjustment banks.
- Added independent Brightness, Contrast, Color, Red, Green, and Blue controls.
- Added an option to link V1 and V2 adjustments.
- Added **Reset both**.
- Saved video display adjustments in the project.
- Included the adjustment values in V-Clip specifications.

## v0.6.164 — Transcript Pop-out Count Repair

- Rewrote transcript pop-out word and utterance count functions.
- Removed problematic template-literal regular-expression escaping that could invalidate the pop-out script.
- Preserved live count updates and pause-marker exclusion behavior.

## v0.6.163 — Word and Utterance Counts

- Added **Words** and **Utts** fields between Duration and Speaker.
- Counts update live as transcript text changes.
- Parenthetical pause-marker content is excluded from both counts.
- Added visibility controls for the new fields alongside Start, Stop, Duration, and Speaker.
