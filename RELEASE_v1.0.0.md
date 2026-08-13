# Phoenix Librarian v1.0.0 — First Stable Release

Phoenix Librarian v1.0.0 is the first frozen stable release of the Windows companion application for the **RTAL Project Phoenix hardware sampler**.

The Librarian provides a visual PC environment for preparing, organizing, validating and auditioning Phoenix content while keeping the hardware sampler completely standalone during normal use.

## Highlights

### Complete Phoenix bank workflow
- Scan and manage Phoenix banks directly from the SD card / USB mass-storage structure.
- Edit four-slot bank content and metadata.
- Work with `BANK.CFG`, `BANK.INFO`, sample WAV files, patterns and song data.
- Search and organize a Factory Library.
- Create backups and perform controlled restores.

### Graphical waveform and loop editor
- Edit `S.START`, `L.START`, `L.END` and `S.END` visually.
- One Shot audition from S.START to S.END.
- Dedicated Loop Hold audition beginning at L.START.
- FORWARD and ALTERNATE loops.
- Crossfade, zero-crossing assistance, trim, DC removal and normalization workflows.

### Quattro routing
- KEYZONE and MULTI workflows.
- Low / Root / High key mapping.
- Per-slot MIDI channel configuration.
- Routing-aware validation.

### Sequencer, song and effects editing
- Pattern editor.
- Song editor.
- Offline pattern/song preview rendering.
- Echo and effects parameter editing and preview.

### MIDI and live audition
- Windows MIDI input through WinMM.
- On-screen keyboard.
- Polyphonic live preview.
- Sustain pedal and pitch bend.
- Native real-time pitch playback from a single cached source sample.

## Audio engine

A significant part of the v1.0.0 development work went into making PC-side audition both responsive and reliable.

The final engine uses:

- WASAPI Event mode
- Exclusive mode with Shared fallback
- 48 kHz / PCM16 stereo
- `LIVE-10ms`: 480-frame buffer for MIDI and screen-keyboard audition
- `EDITOR-20ms`: 960-frame buffer for waveform preview
- MMCSS `Pro Audio`
- fixed 16-voice pool
- preallocated command ring buffer
- GC-isolated render path
- single-source-sample RAM cache
- native pitch and loop processing

The audio endpoint is acquired **on demand**. Phoenix Librarian no longer holds the Windows sound device merely because the application is open. After playback becomes idle or the waveform preview is stopped, the endpoint is released again so other Windows applications can use it.

## Relationship to the Phoenix hardware

Phoenix Librarian is deliberately a **companion editor**, not part of the sampler's required real-time playback chain.

```text
Phoenix hardware sampler
        ↕
SD card / USB mass storage
        ↕
Phoenix Librarian on Windows
```

Banks are prepared and managed on the PC, then used autonomously by Phoenix. The hardware remains a standalone musical instrument.

## Platform

- Windows
- Windows PowerShell 5.1
- WPF
- Native WinMM MIDI
- Native WASAPI audio components embedded through C#

## Starting the Librarian

Extract the release archive and run:

`Start_Phoenix_Librarian.cmd`

Alternatively, start `PhoenixLibrarian.ps1` from Windows PowerShell 5.1 if your local execution policy permits it.

## Release status

**v1.0.0 is the frozen stable release basis.**

Future development should build on this version without changing the validated audio core unless a clearly reproducible issue requires it.

## Notes

Phoenix Librarian can request Exclusive access to the Windows audio endpoint while it is actively producing sound. The v1.0.0 on-demand architecture releases the endpoint again after use instead of occupying it for the entire application session.

Please report reproducible issues together with the relevant Phoenix bank configuration, the action that triggered the problem and the Librarian log where possible.
