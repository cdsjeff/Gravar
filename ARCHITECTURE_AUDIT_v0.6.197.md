# Gravar v0.6.197 Architecture and Efficiency Audit

**Date:** 2026-11-08  
**Build:** Gravar v0.6.197 — Modular Core Refactor

## Purpose

This audit is a deliberate architectural pass rather than a feature addition. The goal is to make Gravar safer and more efficient to continue programming over many future revisions while preserving its defining deployment model: a single standalone HTML application that can also be copied to `index.html` for GitHub Pages/static hosting.

The refactor is intentionally incremental. It introduces clear internal service boundaries and message passing without rewriting the large UI modules all at once.

## Audit scope

The v0.6.196 source was reviewed for:

- module size and responsibility;
- direct cross-module calls and bidirectional coupling;
- duplicated state-transition rules;
- duplicated main-window versus pop-out behavior;
- project serialization and restore paths;
- waveform/media identity and calibration persistence;
- transcript parent/child-tier classification and timing inheritance;
- timer-based coordination and asynchronous restore ordering;
- repeated full-project serialization during editing;
- repeated full-transcript messages to the transcript pop-out;
- late monkey-patching of established modules.

The audit emphasized structural problems that have previously contributed to regressions, rather than changing stable code simply because it is old or large.

## Principal findings

### 1. Gravar already had useful named modules, but cross-cutting rules were duplicated

The v0.6.196 build contained 41 named `X...` modules. This was a strong starting point. The main architectural problem was not the absence of modules; it was that several important rules were implemented in more than one module or UI surface.

Waveform calibration, media identity, child-tier interpretation, parent-timecode inheritance, and project persistence were especially important examples. This type of duplication explains why a change can work in the main transcript but fail in the pop-out, or why project calibration can restore correctly and then be reset by a later media-loading path.

### 2. Several UI modules remain very large

Approximate module ranges before and after this refactor are:

| Module | v0.6.196 | v0.6.197 |
| --- | ---: | ---: |
| `XFiles` | 3,517 lines | 3,493 lines |
| `XTranscript` | 2,838 | 2,789 |
| `XVideo` | 1,956 | 1,924 |
| `XWaveform` | 1,358 | 1,149 |
| `XTranscriptPopout` | 1,986 | 1,963 |
| `XDrawing` | 5,609 | 5,598 |

The reduction in `XWaveform` is meaningful because calibration rules were moved out of the UI module. The other large modules were deliberately not split aggressively in this release; doing so would turn an incremental refactor into a high-risk rewrite.

### 3. Project save behavior depended on monkey-patching `XFiles.payload()`

Drawing and Video Adjustments were extending project serialization by wrapping/replacing `XFiles.payload()` later in the script. That makes project-save behavior dependent on script execution order and encourages every new subsystem to know about the internals of Files.

This has been replaced with an explicit project-state adapter registry.

### 4. A project-load compatibility path depended on a 250-ms timer

A late compatibility patch reapplied video adjustments approximately 250 ms after a project load. That is a race condition: on different machines or with different media, the timer can run before or after other asynchronous restore operations.

The timer was removed. Video adjustments now react to the explicit `project:loaded` message emitted after the asynchronous media restore attempts finish.

### 5. Edit autosave was doing whole-project work too frequently

Many editing paths call `XFiles.autosave()`. Before v0.6.197, an enabled autosave could serialize the entire project, stringify it, and synchronously write browser localStorage on repeated edit events, including typing bursts.

A coordinator now coalesces ordinary edit-triggered recovery saves over a short 600-ms window. Periodic Project-folder backups and final inactivity saves remain immediate and retain their previous semantics.

### 6. Transcript pop-out synchronization duplicated large messages

`XTranscriptPopout.syncSoon()` previously sent a full transcript payload immediately and then sent another full payload about 80 ms later for each call. Rapid edits could therefore generate redundant large cross-window messages.

The new implementation coalesces requests into at most one animation-frame update for a burst plus one settling update after the most recent request.

## Modular core introduced in v0.6.197

### `XBus`

A small application message bus provides `on`, `off`, `once`, `emit`, and coalesced `emitSoon` operations. It is intentionally lightweight rather than a framework.

Current core messages include:

- `media:v1-attached`
- `waveform:calibration-changed`
- `transcript:hierarchy-changed`
- `transcript:hierarchy-settings-changed`
- `project:loaded`

The objective is not to replace every direct function call. Direct calls remain appropriate inside a subsystem or when a synchronous return value is required. Messages are preferred for cross-module notifications where several independent modules need to react to one state transition.

### `XAutosaveCoordinator`

Owns edit-burst scheduling, coalescing, flushing, cancellation, and lifecycle flushing on page hide. `XFiles.performAutosave()` remains responsible for persistence itself. This separates **when to save** from **how to save**.

### `XMediaIdentity`

Owns the rules for recognizing the same primary video after project reload/reconnection. This reduces the chance that calibration-preservation logic and file-loading logic develop different concepts of “same video.”

### `XCalibration`

Owns waveform calibration state transitions and mathematics:

- offset clamping;
- calibration-point normalization;
- interpolation;
- absolute calibration;
- Point addition and Point Undo;
- saved/effective offset calculation;
- legacy drift-anchor and old-offset migration;
- serialization and restoration;
- alignment-state detection.

`XWaveform` remains responsible for waveform UI and drawing, but delegates calibration rules to this service.

### `XTranscriptHierarchy`

Provides one authority for:

- parent/child classification;
- parentheses interpretation;
- italic-Speaker child-tier interpretation;
- parent association;
- collapsed-child visibility;
- Force PT timing inheritance;
- hierarchy settings persistence.

The main transcript is now the mutation authority. The pop-out no longer performs a second independent retagging pass.

### `XProjectState`

Provides a registry of subsystem-owned save/load adapters. It captures registered module state in one pass and writes both:

1. a new structured `moduleState` object; and
2. the established legacy top-level fields required by existing Gravar project files.

Current adapters include:

- calibration;
- transcript hierarchy;
- drawing;
- video adjustments.

Drawing and Video Adjustments therefore no longer monkey-patch `XFiles.payload()`.

### `XCoreMessages`

Contains application-level subscriptions that coordinate modules through messages. This keeps message wiring out of the individual service modules and makes cross-module behavior easier to inspect.

## Project compatibility

v0.6.197 is designed to remain backward compatible with existing Gravar JSON projects.

- Existing top-level calibration and child-tier fields are still written.
- Legacy calibration formats are still migrated on load.
- A new `moduleState` object is additive rather than replacing legacy fields.
- Existing rows, codes, symbols, notes, media references, drawing state, and video-adjustment fields remain available through the established project format.
- The UI and shortcut model were not intentionally changed by this architectural release.

## New architectural diagnostics

The built-in Diagnostics pane now checks the modular core itself. It reports availability of:

- `XBus`;
- `XAutosaveCoordinator`;
- `XMediaIdentity`;
- `XCalibration`;
- `XTranscriptHierarchy`;
- `XProjectState`;
- the calibration, hierarchy, drawing, and video-adjustment project adapters;
- message-bus activity statistics;
- autosave-coalescing statistics;
- separation between autosave scheduling and persistence;
- transcript-pop-out synchronization API availability.

This provides a continuing guardrail against future code accidentally bypassing or removing the new architecture.

## Validation performed

The refactored build passed:

- syntax validation of both embedded JavaScript blocks with `node --check`;
- modular-core regression tests for message coalescing;
- waveform calibration interpolation, Point addition/Undo, and legacy migration;
- child-tier behavior with Child tiers OFF and ON;
- Force PT OFF preserving child timing;
- Force PT ON inheriting parent timing;
- project-state capture/legacy output/restore;
- media-identity matching;
- autosave-coordinator coalescing, metrics, and cancellation;
- static verification that Drawing and Video Adjustments register project-state adapters rather than wrapping `XFiles.payload()`;
- static verification that project completion now emits `project:loaded` rather than relying on the former 250-ms video-adjustment timer.

A full browser end-to-end media test was not available in the current container environment. Headless Chromium did not provide a reliable DOM/media smoke-test result here, so this report does **not** claim that media playback itself was exercised automatically. The build should still be field-tested with representative V1/V2/audio/project files before treating the refactor as a new long-term milestone.

## What was deliberately not rewritten

This build does not attempt to eliminate `XState`, convert every direct module call into an event, or physically split the single HTML into multiple source files. Those changes would substantially increase regression risk and are not necessary to obtain the immediate benefits of clearer state ownership and message passing.

Likewise, the large `XDrawing`, `XFiles`, `XTranscript`, `XVideo`, and `XTranscriptPopout` modules remain substantial. They now have better architectural seams for later extraction.

## Recommended next incremental stages

### Stage 5 — Split persistence responsibilities inside `XFiles`

Separate project serialization, project-file access, imports, exports, and autosave persistence behind small services while keeping the existing Files UI facade. The goal is to make `XFiles` an orchestrator rather than the owner of every file operation.

### Stage 6 — Separate drawing model/state from drawing UI

`XDrawing` is currently the largest module. Extract drawing-object normalization, geometry, serialization, and selection/model operations before changing the rendering/editor layer.

### Stage 7 — Introduce a common change/render coordinator

Gradually replace scattered combinations of `render()`, `autosave()`, and `syncSoon()` with domain change messages. Subscribers can then choose whether a particular mutation requires a main render, waveform redraw, project recovery save, or pop-out synchronization.

### Stage 8 — Reduce remaining duplicated pop-out logic

Keep the pop-out as a separate window, but move more shared transcript rules and payload normalization to main-window services. The pop-out should increasingly become a view/editor transport rather than a second transcript engine.

### Stage 9 — Continue state ownership migration

Move coherent slices of `XState` behind their owning services as each subsystem is touched. Avoid a wholesale state-store rewrite.

## Development rules going forward

For future Gravar changes, the preferred architecture is:

1. one authority for each domain rule;
2. UI modules render and collect input but do not duplicate domain rules;
3. explicit service APIs for synchronous domain operations;
4. `XBus` messages for cross-module notifications and fan-out;
5. `XProjectState` adapters for new persistent subsystem state;
6. no new monkey-patching of established module methods when a registry/API can be used;
7. coalesce high-frequency persistence, rendering, and cross-window messages;
8. preserve legacy project fields until an explicit migration plan exists;
9. validate each refactor stage before moving to the next.

This is the architectural foundation for continuing Gravar development without forcing a rewrite of the application.
