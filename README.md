# CIRCLE OF FIFTHS CLOCK

**English** | [日本語](README.ja.md)

A circle-of-fifths clock that plays a Shepard tone. Three hands follow the current time, and the pitch keeps rising without ever stopping. It is a single-file HTML piece designed for mobile browsers.

A black glass clock and a pair of large stereo speakers are built into a white studio wall. A modular synth sits in front, and a wall monitor shows the sound above.

---

## How to use

1. Open `index.html` in a browser. If you publish it with GitHub Pages, opening the URL is all it takes.
2. Tap the **PLAY** button on the synth to start the sound. Tap it again to stop.
   - The first time you open the page, a **TAP TO PLAY** bubble appears above the button. Tapping the bubble also starts playback.
   - Browsers do not allow sound until you tap the screen once.
   - The ring around the button is green while playing and red (slowly pulsing) while stopped.
   - Starting and stopping fade the sound in and out over 0.3 seconds.
3. Tap the synth panel to zoom in on that module and use its knobs and switches.
   - Swipe sideways to move to the next module.
   - Tap **BACK** to return to the full view.
4. Tap the wall monitor to open the detailed meter view (**SCOPE**).
5. Tap the digital clock at the top left to pick a time and jump to it. Press **NOW** to return to the current time.

The camera is fixed to the front view. You cannot move the viewpoint with your finger.

### Requirements

- A browser that supports WebGL2 and Web Audio (recent Chrome, Safari, Edge or Firefox on iPhone, Android or desktop)
- Use earphones or stereo speakers to hear the sound move left and right with the hands

---

## How the sound works

### The circle of fifths is a scale on the dial

The note names are laid out around the dial in circle-of-fifths order:

`C – G – D – A – E – B – F# – C# – Ab – Eb – Bb – F`

A note does not only sound when a hand points at its name. The frequency is interpolated smoothly between names, so **the pitch always keeps rising**. For example, between C and G the pitch passes through the points that divide the C–G interval at 1:4, 2:3 and so on.

### Shepard tone

Each voice is built from seven components spaced an octave apart. The highest components fade away while new ones enter softly from below. This creates the illusion of a pitch that rises forever without leaving its range.

The reference pitch is fixed at **A4 = 440 Hz**.

### What the three hands do

| Hand | Length | Role |
|---|---|---|
| Second hand | Longest | The tone you hear. Choose **TICK** (sounds once per second) or **DRONE** (sounds continuously) |
| Minute hand | Medium | Applies **AM (amplitude modulation)** to the second-hand tone |
| Hour hand | Shortest | Also applies **AM** to the second-hand tone |

AM makes the volume swell and dip periodically. The swing is centred on the original level, so **the fundamental is always preserved**.

- **AUDIO** (default): modulates at the pitch of the minute and hour hand positions (audio-rate frequencies). This changes the timbre itself and gives a metallic sound.
- **SLOW**: a slow tremolo of a few Hz or less.

### Panning

Left–right placement is calculated from the position of each hand tip relative to the camera. A hand on the right side of the clock sounds from the right speaker, and one on the left sounds from the left. Each of the three hands is panned separately.

---

## Reading the screen

| Area | What it shows |
|---|---|
| Top left | Digital clock and the note each hand is pointing at (`HOUR · MIN · SEC`). Tap it to pick a time |
| Top right | Title |
| Wall monitor | From left: **SPECTROGRAM** (frequency over time), **SPECTRUM** (level by frequency), **WAVEFORM** |
| Clock | The LEDs light up where the hands pass |
| Synth in front | Control panel (see the tables below) |
| **▴ METERS** at the bottom | A drawer with small meters. It starts closed |

### The METERS drawer

- **▶ PLAY**: start / stop
- **SCOPE**: open the detailed meter view
- **CONTROL**: zoom in on the synth controls
- **▾**: close the drawer

### SCOPE (detail view)

Shows the three monitor displays in a large view with scales and numbers. Tap **CLOSE** to close it.

---

## Synth control panel

Five modules sit side by side. The leftmost is the logo panel; the other four have controls.

### Logo (leftmost)

A panel with the **1void_** logo and the title **Circle of Fifths Clock**, set in Inter. It has no controls.

### SECONDS

| Control | What it does | Range / default |
|---|---|---|
| Switch **TICK / DRONE** | How the second hand sounds | Default: DRONE |
| Knob **DECAY** | Length of the tail in TICK mode | 0.08–1.5 s / 0.5 s |

### AM

| Control | What it does | Range / default |
|---|---|---|
| Knob **HOUR** | Depth of the hour-hand AM | 0–100% / 60% |
| Knob **MIN** | Depth of the minute-hand AM | 0–100% / 60% |
| Switch **SLOW / AUDIO** | AM speed | Default: AUDIO |

### TRANSPORT

| Control | What it does | Range / default |
|---|---|---|
| **PLAY** (large button) | Start / stop (green ring = playing, red = stopped) | — |
| **NOW** | Return a jumped time to the current time (speed is kept) | — |
| Knob **SPEED** | How fast time runs | ×1–×120 / ×1 |
| Knob **JOG** | Move the time forward or back. Drag up to go forward, down to go back (about 180 px = 1 hour) | — |
| Display | The speed and the time shown on the clock | — |

- SPEED moves in finer steps at low values so that settings around ×60 are easy to reach. Faster speeds fast-forward both the hands and the rising pitch.
- The red mark on JOG makes one full turn per hour, like the minute hand.
- To jump far in one go, tap the digital clock at the top left and pick a time.

### MONITOR

Shows each hand's position, frequency and pan as numbers. Cables run from the three jacks at the bottom to the wall.

---

## URL option

| Example | What it does |
|---|---|
| `index.html?t=10:08:30` | Freezes the hands at the given time (for screenshots) |

---

## Technical notes

- **Single file**: `index.html` contains the shaders, the audio engine and the fonts for the note names and logo. No server is needed. Only the panel headings and numbers (Barlow Condensed / Share Tech Mono) load from Google Fonts; offline, a fallback font is used.
- **Graphics**: the whole studio is drawn by one WebGL2 raymarching (signed distance field) shader.
- **Keeping it light**: because the camera is fixed, the static parts are drawn once when the page opens and stored. Each frame only shows that stored image and redraws the parts that move:
  - the clock hands and LEDs
  - the monitor image
  - the panel you touch
- **Cables**: their shape is precomputed as ropes sagging under gravity and embedded in the page.
- **Note names**: Inter glyphs are converted into a distance-to-outline image, embedded, and raised by the shader into metal letters with a rounded cross-section.
- **Audio**: Web Audio API. The chain is Shepard tone (7 components) → hour/minute AM → panning → limiter (to prevent clipping) → start/stop fade (0.3 s).

---

## Credits

- Work: 1void_
- Note-name and logo font: [Inter](https://rsms.me/inter/) (Rasmus Andersson / SIL Open Font License 1.1)
- Panel fonts: [Barlow Condensed](https://fonts.google.com/specimen/Barlow+Condensed), [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) (Google Fonts / SIL Open Font License 1.1)
