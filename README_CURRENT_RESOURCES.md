# Gravar v0.6.187 Current Resources

This package contains the current standalone Gravar web application and repository-ready resources.

- `index.html` — current Gravar application for GitHub Pages/static hosting
- `Gravar_v0.6.187_Project_Folder_Reconnect.html` — versioned standalone application
- `CHANGELOG.md` — revision history by version
- `CHANGELOG_BY_DATE.md` — revision history organized by date
- `Gravar_v0.6.187_SHA256.txt` — checksums

Current release: **v0.6.187** adds Project-folder reconnection and media recovery. The Reconnect button remains hidden unless a loaded project remembers a Project folder that Gravar cannot access. After permission is renewed, Gravar searches the approved folder for the saved V1, V2, and external-audio filenames and restores any files that are present while preserving saved waveform calibration.

For the most reliable persistent Project-folder workflow on macOS, use the same hosted Gravar URL in Chrome or Edge. Safari continues to use the Save As workflow because it does not provide the same writable-directory access.
