# Bell Chime — iPhone Edition

A meditative bellchime synthesizer PWA built for iPhone. FM synthesis inspired by the Yamaha DX-7 TUBBELL algorithm, with real-time time-stretching, pitch shifting, and convolution reverb.

## Features

- **Tap the bell** to ring it — large, tactile centerpiece with light/shadow gradient and ripple animation
- **5 rotary knobs** — drag vertically to adjust, double-tap to reset
  - **Stretch** (0.25× – 4×): time-stretches the bell decay
  - **Pitch** (±24 semitones): transposes the entire sound
  - **Tone**: controls brightness via FM modulation depth + lowpass filter
  - **Space**: convolution reverb amount
  - **Level**: master volume
- **Live waveform** visualizer at the bottom
- **Haptic feedback** on taps and knob changes
- **True FM synthesis** with 6 voice pairs (carrier + modulator), just like the DX-7
- **Algorithmic reverb** via generated impulse response
- Works **fully offline** after first load

## iPhone-specific design

- Respects safe-area insets (notch, Dynamic Island, home indicator)
- Standalone mode — no browser chrome when installed
- Status bar set to translucent black
- Prevents pinch-zoom and double-tap zoom
- Portrait-locked with proper viewport handling
- Install hint on first visit (iOS Safari doesn't have automatic install prompts)
- Touch-first interaction model with pointer events

## How to install on iPhone

1. Host the three files on a web server (HTTPS required):
   - `index.html`
   - `manifest.json`
   - `service-worker.js`

2. Open the URL in **Safari** on your iPhone

3. Tap the **Share** button (square with up arrow)

4. Scroll down and tap **Add to Home Screen**

5. Tap **Add** — the app now appears on your home screen

6. Launch from home screen — runs full-screen with no browser UI

## Quick local test

```bash
# In the folder with the files:
python3 -m http.server 8000

# On your iPhone, open:
# http://<your-computer-ip>:8000
```

For real PWA features (service worker, install), deploy to any HTTPS host:
- Netlify (drag-and-drop the folder)
- Vercel
- GitHub Pages
- Cloudflare Pages

## How to use

- **Tap the bell** in the center to ring
- **Drag knobs up** to increase values, **down** to decrease
- **Double-tap any knob** to reset to default
- Tap **Reset** in the bottom-right to reset everything

## Audio design notes

The DX-7's famous TUBBELL patch uses FM synthesis with inharmonic frequency ratios to create bell-like partials. This implementation stacks 6 carrier+modulator pairs with ratios like 3.5:1, 7:1, and 14:1 — these non-integer ratios are what give bells their characteristic shimmer that pure sine harmonics can't produce.

Each voice has:
- A fast attack (3ms) and exponential decay (scaled by Stretch)
- Its own modulator envelope that decays faster than the carrier (creates the bright attack that softens as it rings out)
- Slight detuning for chorus-like width
- A lowpass filter shaped by the Tone knob

The reverb uses a generated impulse response (3.5 seconds, exponential decay) processed through a ConvolverNode.

## Browser support

- iOS Safari 14+ ✓
- Chrome/Edge 80+ ✓
- Firefox 76+ ✓
- Samsung Internet ✓

## Files

- `index.html` — the entire app (HTML + CSS + JavaScript, no dependencies)
- `manifest.json` — PWA metadata and icons
- `service-worker.js` — offline caching
