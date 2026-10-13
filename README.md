# ReaperFX

JSFX effects for REAPER by sguyader, installable with ReaPack.

| Effect | Category | What it is |
|---|---|---|
| [A-Lift](#a-lift) | Dynamics | Four-band "encode-only" lift: brings up low-level detail and air |
| [P-Clip](#p-clip) | Dynamics | Soft clipper modelled on StandardCLIP's Soft Clip Pro mode |

The effects need the tk-gui-tools library, which you install yourself: see
[GUI library](#gui-library-required).

## Install with ReaPack

In REAPER: Extensions → ReaPack → Import repositories, and paste

```
https://github.com/sguyader/ReaperFX/raw/main/index.xml
```

Then Extensions → ReaPack → Browse packages, and install the effects you want.
ReaPack does not install the [GUI library](#gui-library-required); add it
yourself.

## Manual install

Each effect lives in its category folder (for example `Dynamics/`). Copy the
`.jsfx` file into your REAPER `Effects` folder (Options → Show REAPER resource
path), add the [GUI library](#gui-library-required), then rescan.

## GUI library (required)

The GUIs are drawn with the tk-gui-tools JSFX library. It is not distributed
here; download it from the REAPER stash:

https://stash.reaper.fm/v/53055/tk-gui-tools.zip

Unzip it and copy `tk_gui_tools.jsfx-inc` anywhere inside your REAPER `Effects`
folder (Options → Show REAPER resource path). The rest of the zip (examples,
images, documentation) is not needed.

Without that file an effect shows `import: can't find file
'tk_gui_tools.jsfx-inc'` instead of its GUI. Once the file is in place, remove
the effect and add it again.

## A-Lift

File: `Dynamics/A-Lift.jsfx`

A four-band "encode-only" lift, in the spirit of the old studio trick of
running a vocal or acoustic source through an A-type noise-reduction encoder
without decoding it. It is a behavioural model of the effect, not a circuit
emulation of any specific unit, and it is not affiliated with Dolby
Laboratories.

### How it works

The input is split into four bands with phase-coherent Linkwitz-Riley
crossovers: below 80 Hz, 80 Hz – 3 kHz, above 3 kHz, and above 9 kHz (which
overlaps the third band).

Each band drives a side chain: an envelope follower, a soft-knee limiter and a
soft clipper. The side chains are added back to the signal. Quiet material gets
about +10 dB in-band (about +14 dB at the very top). As the signal rises above
Threshold the lift fades away, so low-level detail, breaths, room tone and the
top end come up while peaks barely change.

### Controls

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

## P-Clip

File: `Dynamics/P-Clip.jsfx`

A soft clipper modelled on the Soft Clip Pro mode of SIR Audio Tools'
StandardCLIP, with its input gain, clip level and Soft-Clip Saturator slider.
The clip curve was worked out from the description and function plots in the
StandardCLIP manual; the plugin is not affiliated with SIR Audio Tools.

### How it works

Input Gain is applied first, to drive the signal into the clip. Clip Level
sets where the clip sits, from 0 down to −24 dBFS. Saturation sets an amount
K, from 0 dB at 0 % to 12 dB at 100 %:

- more than 2K dB below the clip level, the signal is only raised by K dB;
- the last 2K dB below the clip level are squeezed into K dB by a soft knee
  (output dB = −(input dB)² / 4K);
- at the clip level and above, the signal is hard-clipped.

| Saturation | What it does |
|---|---|
| 0 % | Hard clip at the clip level, nothing else |
| 25 % | Compresses the last 6 dB into 3 dB, and adds 3 dB |
| 50 % | Compresses the last 12 dB into 6 dB, and adds 6 dB |
| 75 % | Compresses the last 18 dB into 9 dB, and adds 9 dB |
| 100 % | Compresses the last 24 dB into 12 dB, and adds 12 dB |

Not modelled: StandardCLIP's output gain, ceiling, other clip modes and
oversampling. Without oversampling, hard-driven material aliases more
than it would there. Lowering Clip Level lowers the output peaks with it;
make up the level after the plugin.

### Controls

| Control | What it does |
|---|---|
| Input Gain | −12 to +24 dB. Gain before the clip, to push the signal into it. |
| Clip Level | −24 to 0 dBFS. The level the signal is clipped at; the knee sits just below it. |
| Saturation | 0–100 %. From a plain hard clip to the widest knee and the most added gain. |

The plot shows the input/output curve from −24 to +6 dBFS, with the knee and
clip zones shaded and a thin line at the output level where the knee starts;
the dot is where the signal, after Input Gain, sits on the curve right now.
The GR meter shows how much the knee and the clip take off after the gain
Saturation adds, on a 0–12 dB scale. A light marker holds the recent maximum
for 1.5 s, and the box under the meter shows the largest reduction since you
last clicked the meter, including values beyond 12 dB; it is kept when
playback stops and starts. Under the knobs: knee width, the gain Saturation adds below
the knee (SAT GAIN) and the level where the knee starts (threshold).

Knobs: drag to adjust (Shift for fine), mouse wheel, double-click to reset.
Click the value under a knob to type a number (Enter applies, Esc cancels).
The window is resizable and the GUI scales with it.

## Credits

GUIs are drawn with the tk-gui-tools JSFX library (`tk_gui_tools.jsfx-inc`),
which is free to use with free JSFX plugins; see the notice at the top of that
file.

## License

MIT (see `LICENSE`). The tk-gui-tools library is not part of this repository
and has its own terms.
