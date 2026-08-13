# Phoenix Sampler Integration

Phoenix Librarian is designed as a companion to the RTAL Project Phoenix hardware sampler, not as a replacement for it.

## Division of responsibilities

### Phoenix hardware
- real-time standalone sample playback
- MIDI performance
- embedded audio processing
- hardware controls and display
- SD-based sample and bank storage

### Phoenix Librarian
- visual bank management
- sample and loop editing
- routing preparation
- pattern and song editing
- effects setup
- metadata and library organization
- backups and restores
- PC-side audition and MIDI mapping checks

The PC therefore remains outside the hardware sampler's real-time playback dependency.

## Storage workflow

A typical Phoenix media layout contains a `PHOENIX` directory with bank folders. A bank can include files such as:

```text
PHOENIX/
  BANKS/
    BANK01/
      BANK.CFG
      BANK.INFO
      SLOT1.WAV
      SLOT2.WAV
      SLOT3.WAV
      SLOT4.WAV
      PATTERNS.CFG
      SONG.CFG
```

The exact contents depend on the bank and Phoenix firmware revision.

Phoenix Librarian treats the files on the Phoenix media as the authoritative data. The user edits those files through the Librarian and Phoenix subsequently reads them on the hardware.

## Sample markers

The waveform editor uses four important boundaries:

- `S.START` — sample playback start
- `L.START` — loop start
- `L.END` — loop end
- `S.END` — sample playback end

### One Shot

The visual playhead and audition start at `S.START` and continue toward `S.END`.

### Loop Hold

The dedicated loop audition starts visually at `L.START` and follows the loop region. FORWARD returns to L.START after L.END; ALTERNATE reverses direction at the loop limits.

## Quattro routing

Phoenix banks can use different Quattro routing concepts. The Librarian exposes the relevant routing data instead of forcing one interpretation on every bank.

### KEYZONE
The four slots can be mapped by Low / Root / High keyboard values.

### MULTI
Slots can be addressed using their configured MIDI channels. Root-note validation therefore differs from KEYZONE mode and is not treated as a key-zone error merely because a root note lies outside a zone that is not active in MULTI mode.

## USB mass storage and SD card use

Phoenix can expose its storage to Windows through USB mass storage, or the SD card can be mounted directly. The Librarian operates on the same bank structure in either case.

Before physically removing media, use the normal Windows safe-removal workflow and ensure Phoenix is no longer writing to it.
