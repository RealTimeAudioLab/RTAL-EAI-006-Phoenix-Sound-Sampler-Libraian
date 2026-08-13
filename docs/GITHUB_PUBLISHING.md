# GitHub Publishing Checklist

## Repository

Recommended repository name:

`Phoenix-Librarian`

Suggested description:

> Windows librarian, sample editor, sequencer editor and low-latency audition environment for the RTAL Project Phoenix hardware sampler.

## Suggested topics

`sampler`, `esp32-s3`, `midi`, `audio-dsp`, `sample-editor`, `librarian`, `powershell`, `wpf`, `wasapi`, `realtime-audio`, `embedded-audio`, `music-technology`

## Before the first public push

- Add project screenshots to `images/` and replace the README image placeholders.
- Choose and add a software license.
- Verify that the example bank contains only material that may be redistributed.
- Check the repository for personal paths, private sample names or machine-specific information.
- Keep the release ZIP in GitHub Releases rather than committing it to the normal source tree.

## Recommended image set

1. Main Phoenix Librarian window.
2. Waveform editor with S.START / L.START / L.END / S.END visible.
3. KEYZONE / MULTI routing view.
4. Pattern editor.
5. Song editor.
6. MIDI / screen keyboard.
7. Backup & Restore Center.
8. Phoenix hardware next to the Librarian running on the PC.

A short animated GIF showing bank selection, loop editing and playback would make the project page significantly easier to understand at a glance.

## Release

Create a GitHub Release with tag:

`v1.0.0`

Attach:

`Phoenix_Librarian_v1_0_0_Final.zip`

Use `RELEASE_v1.0.0.md` from the repository root as the release description.
