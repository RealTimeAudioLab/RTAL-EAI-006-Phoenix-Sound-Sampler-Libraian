# Changelog

All notable public changes to Phoenix Librarian are documented here.

## 1.0.0 — Stable Release Basis

Phoenix Librarian v1.0.0 is the first frozen stable release basis for the Windows companion application of the RTAL Project Phoenix sampler.

### Bank and library management
- Scan and load Phoenix bank structures directly from the Phoenix SD / USB mass-storage volume.
- Four-slot bank overview with sample names, metadata and status information.
- Bank metadata support through `BANK.INFO`.
- Full-text Factory Library search and filtering.
- Factory Library export with renumbering and generated index files.
- Backup and Restore Center with safety backup before restore.

### Waveform editor
- Graphical sample display.
- S.START, L.START, L.END and S.END marker editing.
- One-shot audition from S.START to S.END.
- Loop Hold audition with playhead starting at L.START.
- FORWARD and ALTERNATE loop modes.
- Loop crossfade support.
- Zero-crossing assistance.
- Trim, DC removal and normalization preview workflows.

### Phoenix routing
- Quattro KEYZONE and MULTI modes.
- Per-slot MIDI channel handling.
- Low / Root / High keyboard mapping.
- Validation of routing and root-note relationships according to the active mode.

### Sequencing and effects
- Pattern editor.
- Song editor.
- Pattern repeat preview.
- Offline pattern and song rendering.
- Echo / effects editing and preview.

### MIDI and live preview
- Windows MIDI input via WinMM.
- Screen keyboard.
- Polyphonic live audition.
- Sustain pedal handling.
- Pitch bend.
- Native pitch playback without generating pitch-copy WAV files.

### Audio architecture
- Native WASAPI event-driven preview engine.
- Exclusive mode with Shared fallback.
- LIVE profile: 48 kHz, PCM16 stereo, 480 frames / 10 ms.
- EDITOR profile: 48 kHz, PCM16 stereo, 960 frames / 20 ms.
- MMCSS `Pro Audio` scheduling.
- Fixed 16-voice pool.
- Preallocated command ring buffer.
- GC-isolated real-time render path.
- Single-source-sample RAM cache.
- On-demand Exclusive audio acquisition.
- Automatic endpoint release when Phoenix audio is idle or preview stops.

### Diagnostics and reliability
- Self Test covering Phoenix path, BANKS, parsers, WAV decoding, write access, backup path, MIDI and audio preview.
- Audio health counters and timing diagnostics retained for troubleshooting.
- Clean WASAPI shutdown and device-in-use recovery.

### Final UI adjustment
- One Shot playhead starts at S.START.
- Loop Hold playhead starts at L.START and follows the selected loop mode.

