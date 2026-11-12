# Gravar v0.6.200 Current Resources

Current standalone Gravar build with the modular core architecture, expanded default Symbols palette, verified manual Project Save location workflow, and long-video waveform calibration fix.

## Files

- `index.html` — current GitHub/static-hosting copy of Gravar v0.6.200.
- `Gravar_v0.6.200_Long_Video_Calibration.html` — same standalone application with descriptive filename.
- `CHANGELOG.md` — revision history.
- `CHANGELOG_BY_DATE.md` — revision history organized by date.
- `ARCHITECTURE_AUDIT_v0.6.197.md` — architecture/efficiency audit that established the current modular/message-passing direction.
- `gravar-shared-symbols.json` — expanded built-in/default Symbols set.
- `SHA256SUMS.txt` — checksums for packaged resources.

## v0.6.200 focus

Waveform calibration on long recordings no longer routes saved Point offsets through the temporary ±4-second Wave± clamp. The temporary nudge remains bounded, while cumulative saved calibration can exceed ±4 seconds and remain effective beyond 30 minutes.
