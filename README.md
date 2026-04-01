# Sprint Timer

A minimal, typographically-driven countdown timer built as a single self-contained HTML file. Designed for focused work sessions (sprints, pomodoros, timeboxing), it displays time remaining as four large digits in a 2x2 grid layout.

## How It Works

Open `timer.html` in any modern browser — no build step, no dependencies, no server required.

### Display

The timer splits minutes and seconds into four individual digits arranged in a quadrant grid:

```
 ┌───────┬───────┐
 │       │       │
 │   0   │   5   │   ← minutes (tens, ones)
 │       │       │
 ├───────┼───────┤
 │       │       │
 │   0   │   0   │   ← seconds (tens, ones)
 │       │       │
 └───────┴───────┘
```

Digits animate with a blur/scale morph transition when they change. A thin progress bar runs along the top of the viewport.

### Controls

The control bar sits at the bottom of the screen and fades out while the timer is running (reappears on hover).

- **Presets**: Quick-select buttons for 5, 10, and 25 minute durations
- **Custom input**: Editable minute:second fields (up to 99:59)
- **Start / Pause / Resume**: Single button toggles state
- **Reset**: Returns to the entered duration

### Keyboard Shortcuts

| Key     | Action              |
|---------|---------------------|
| `Space` | Start / Pause       |
| `R`     | Reset               |
| `D`     | Toggle dark mode    |

### Urgency Cues

As time runs low, the display shifts to communicate urgency:

- **Under 60 seconds**: Digits turn accent-colored and italic
- **Under 10 seconds**: Digits become bold italic with a per-second audio pulse
- **At zero**: Digits flash three times and a rising chime sequence plays

### Audio

Sound is generated entirely with the Web Audio API (no audio files). Effects include:

- A three-tone chord on start
- A single tone on each minute boundary
- Escalating pulses in the final 10 seconds
- A four-note ascending chime on completion

### Dark Mode

Supports light and dark themes. Defaults to the system preference and can be toggled via the sun/moon button or the `D` key.

## How It's Built

The entire application is a single `timer.html` file (~400 lines) with no external dependencies beyond two Google Fonts:

- **Playfair Display** (serif) — used for the large digit display and time inputs
- **Karla** (sans-serif) — used for labels, buttons, and UI controls

### Architecture

Everything lives inline — HTML structure, CSS, and JavaScript are all in one file:

- **CSS custom properties** define a token system (`--bg`, `--fg`, `--accent`, etc.) with a `.dark` variant on `:root`, enabling theme switching by toggling a single class
- **Visual texture** is added through a radial vignette (`body::before`) and SVG-based film grain (`body::after`) — both purely decorative CSS layers
- **Layout** uses CSS Grid for the four-quadrant digit display and flexbox for the control bar
- **Timer logic** is a straightforward `setInterval` at 1-second resolution, tracking `total` and `rem` (remaining) seconds
- **Digit morphing** triggers a CSS keyframe animation (`luxBloom`) that blurs and scales the digit out, swaps the text content at the midpoint, then animates back in
- **Audio synthesis** creates oscillator nodes on the fly via the Web Audio API — each sound is a short function composing `tone()` calls with frequency, waveform, duration, gain, and delay parameters
