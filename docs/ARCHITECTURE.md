# Phoenix Librarian Architecture

Phoenix Librarian combines a PowerShell/WPF application layer with embedded C# components for the parts that require deterministic native Windows access.

## High-level structure

```text
WPF UI
  |
  +-- PowerShell application state
  |     +-- bank scanning
  |     +-- configuration parsing
  |     +-- metadata
  |     +-- import/export
  |     +-- backup/restore
  |     +-- pattern/song/effects workflows
  |
  +-- Embedded C# modules
        +-- WinMM MIDI input
        +-- WASAPI audio engine
        +-- native pitch / loop mixer
        +-- timing and health counters
```

## Why PowerShell + WPF

The Librarian was intentionally kept inspectable and portable. PowerShell 5.1 provides the file-system and automation layer while WPF provides a native Windows desktop UI. Time-critical work is moved out of PowerShell and into embedded C#.

## Audio design evolution

The preview system evolved through several architectures:

1. WPF `MediaPlayer` for simple preview.
2. Pre-rendered pitched WAV caches to obtain correct pitch.
3. Segmented attack/hold/release preview experiments.
4. Native WASAPI shared-mode playback.
5. WASAPI Exclusive/Event playback with lower latency.
6. Native pitch from a single source sample in RAM.
7. Stable dual audio profiles: LIVE 10 ms and EDITOR 20 ms.
8. Fixed voice pool and preallocated command ring buffer to remove managed allocation pressure from the real-time path.
9. On-demand Exclusive mode so Phoenix does not hold the Windows audio endpoint while idle.

The final v1.0.0 design prioritizes repeatable stability over the smallest technically possible buffer size.

## Real-time audio core

The audio engine uses:

- 48 kHz output
- PCM16 stereo
- WASAPI Event mode
- Exclusive mode when available
- Shared mode fallback
- MMCSS `Pro Audio`
- a fixed 16-voice pool
- a preallocated command ring buffer
- a preallocated PCM work buffer
- interpolation and pitch stepping in the native mixer
- no disk reads during Note On after the source sample has been cached

### Profiles

`LIVE-10ms` is optimized for MIDI and screen-keyboard responsiveness.

`EDITOR-20ms` provides additional scheduling margin for continuous waveform audition and looping.

## On-demand endpoint ownership

Exclusive WASAPI is not opened merely because the application or a Phoenix SD card is present. The endpoint is acquired only when Phoenix actually needs to produce sound. It is released again after preview stops or after the live engine has been idle for a short period.

This prevents Phoenix Librarian from unnecessarily blocking Windows Media Player, a DAW or another application while the Librarian is being used only for file editing.
