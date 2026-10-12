# A-Lift (JSFX)

A four-band "encode-only" lift effect for REAPER, in the spirit of the old
studio trick of running a vocal or acoustic source through an A-type
noise-reduction encoder without decoding it.

It is a behavioural model of the effect, not a circuit emulation of any specific
unit, and it is not affiliated with Dolby Laboratories.

## Install with ReaPack

In REAPER: Extensions → ReaPack → Import repositories, and paste

```
https://github.com/sguyader/ReaperFX/raw/main/index.xml
```

Then Extensions → ReaPack → Browse packages, and install **A-Lift**.

## Manual install

Copy both files from `Dynamics/` (`A-Lift.jsfx` and `tk_gui_tools.jsfx-inc`)
into the same folder inside your REAPER `Effects` folder (Options → Show REAPER
resource path), then rescan.

## How it works

The input is split into four bands with phase-coherent Linkwitz-Riley
crossovers: below 80 Hz, 80 Hz – 3 kHz, above 3 kHz, and above 9 kHz (which
overlaps the third band).

Each band drives a side chain: an envelope follower, a soft-knee limiter and a
soft clipper. The side chains are added back to the signal. Quiet material gets
about +10 dB in-band (about +14 dB at the very top). As the signal rises above
Threshold the lift fades away, so low-level detail, breaths, room tone and the
top end come up while peaks barely change.

## Controls

| Control | What it does |
|---|---|
| Threshold | Level where each band's lift starts to fade. Lower = only quiet detail is lifted; higher = more of the signal gets lifted. |
| Lows | Amount of the two bands below 3 kHz. Set to 0 for the "air only" version of the trick. |
| Highs | Amount of the two bands above 3 kHz. |
| Speed | Scales all attack/release times (0.5x–2x). Faster = more transient bite and more movement. |
| Character | Side-chain clip drive. Low = clean; high = more edge on transients. |
| Mix | How much of the side chains is added back. 100 % = full encode; lower = parallel. |
| Output | Make-up / trim. The effect adds level, so pull this down to compare fairly. |
| LINK THR | While on, turning Threshold trims Output by the level change the lift is estimated to cause on the audio currently playing, so loudness stays roughly constant. Works on knob moves in the GUI, not on host automation. |

The meter shows how much lift each band is adding at that moment.

Knobs: drag to adjust (Shift for fine), mouse wheel, double-click to reset.
Click the value under a knob to type a number (Enter applies, Esc cancels).
The window is resizable and the GUI scales with it.

## Credits

The GUI is drawn with the tk-gui-tools JSFX library
(`Dynamics/tk_gui_tools.jsfx-inc`), which is free to use and share with free
JSFX plugins; see the notice at the top of that file.

## License

MIT (see `LICENSE`) for everything except `tk_gui_tools.jsfx-inc`, which
remains under its own terms.
