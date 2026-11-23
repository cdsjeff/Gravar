# Gravar Current Resources — v0.6.209

Current standalone build: `Gravar_v0.6.209_Project_Save_Media_Stall_Repair.html`.

This release repairs a Project Save media-state problem that could unnecessarily rewrite a large V1 video file and interfere with playback. Gravar now verifies and reuses matching media already present in the active Project folder, preserves folder-restored media state, and copies a genuinely missing V1 only once.

`index.html` is an identical copy of the named standalone HTML for GitHub/web deployment.
