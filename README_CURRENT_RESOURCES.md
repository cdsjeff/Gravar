# Gravar Current Resources — v0.6.211

Current standalone build: `Gravar_v0.6.211_Merge_Line_Note_Repair.html`.

This release repairs replicated Line Note loss during Merge. Merge now commits/closes any open inline Line Note before rebuilding rows and no longer calls the commit-first `selectOne()` path after the merged row becomes active, preventing stale side-pane Line Note or Code content from overwriting the merged data. Project-home state repair and Project Save media-reuse protections from v0.6.210/v0.6.209 are retained.

`index.html` is an identical copy of the named standalone HTML for GitHub/web deployment.
