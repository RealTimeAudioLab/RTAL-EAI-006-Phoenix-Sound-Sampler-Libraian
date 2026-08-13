# Development History

Phoenix Librarian v1.0.0 is the result of an iterative development process in which functionality and audio reliability were measured and refined rather than being treated as a single monolithic implementation.

## Early editor stages

The first stages concentrated on Phoenix bank parsing, four-slot sample management and editing the sampler's configuration files. The UI was progressively expanded with waveform visualization, loop points, routing parameters, pattern/song data and backup workflows.

## Waveform and loop editing

The waveform editor evolved into a dedicated sample-preparation environment with S.START, L.START, L.END and S.END, zero-crossing assistance, normalization/DC workflows and FORWARD/ALTERNATE loop audition.

A final v1.0.0 UI rule was frozen:
- One Shot playhead begins at S.START.
- Loop Hold playhead begins at L.START.

## Pattern / song preview

Early WPF `MediaPlayer` step playback produced clicks and unreliable timing. Pattern and song preview therefore moved to offline rendering. This gave deterministic timing and eliminated the discontinuities caused by repeatedly starting independent player instances.

## Live preview and pitch

The live preview first used temporary WAV files and WPF MediaPlayer. Pitch through `SpeedRatio` proved unsuitable, so pitched sample caches were generated. This worked but became memory-heavy when long HOLD files were pre-rendered for many notes.

The architecture was replaced with a native pitch engine: one original source sample is held in RAM and the playback step is varied in the mixer. This removed the need for per-note pitch files.

## WASAPI work

Shared-mode WASAPI improved control of the audio path but the Windows endpoint still exposed a relatively large effective buffer. Exclusive/Event mode was then developed and the endpoint was probed for supported formats.

The tested device accepted PCM16 stereo at 48 kHz. Very small 3 ms buffers were achievable, but long-run tests demonstrated scheduling sensitivity. The stable release therefore uses:

- LIVE: 480 frames / 10 ms
- EDITOR: 960 frames / 20 ms

## Dropout investigation

A sequence of diagnostic builds measured event wakeups, mixer time, write time and complete render-loop time. The mixer itself was consistently very fast, while managed runtime stalls could cause much larger timing excursions.

The final solution was not simply to keep increasing the buffer. Instead, the audio core was redesigned to reduce managed-runtime interference:

- fixed voice pool
- preallocated command ring buffer
- no dynamic voice creation in the render loop
- preallocated output buffers
- deferred diagnostics
- GC-isolated render architecture

This eliminated the audible dropouts in the validated v1.0.0 test path.

## Audio endpoint ownership

Keeping an Exclusive WASAPI endpoint open from application startup prevented other Windows applications from playing sound. The v1.0.0 architecture therefore acquires Exclusive audio only when Phoenix actually needs playback and releases it again after use.

This is a good example of the project's design philosophy: low latency is useful, but good desktop integration and predictable behavior are equally important.
