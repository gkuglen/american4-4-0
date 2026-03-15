# 🚂 The American 4-4-0 — WebAR Experience
## Setup, Customization & Deployment Guide

---

## What's in this folder

```
historic-train-ar/
├── index.html        ← The AR experience (A-Frame + AR.js)
├── qr-generator.html ← Generate & print your QR code
├── train.glb         ← DROP YOUR MODEL HERE (rename to train.glb)
└── README.md         ← This file
```

---

## Step 1 — Get a train model

Download a free GLB from one of these sources:

| Source | URL | Notes |
|--------|-----|-------|
| Sketchfab | sketchfab.com/tags/steam-locomotive | Filter: "Downloadable", format: GLB |
| Get3DModels | get3dmodels.com/vehicles/low-poly-train | Already GLB, free |
| CGTrader | cgtrader.com/3d-models/steam-train | Filter: Free + GLB |

- Rename your downloaded file to **train.glb**
- Place it in the same folder as index.html

**Tips for choosing a model:**
- Under 10MB is ideal for mobile loading
- "Low poly" models load faster and look great in AR
- Look for models with "animations included" if you want wheel spin etc.

---

## Step 2 — Set your GPS coordinates

Open `index.html` and find this section (look for the ╔═══╗ comment boxes):

```html
<a-entity
  id="train-anchor"
  gps-entity-place="latitude: 37.7879; longitude: -122.4075">
```

**Replace 37.7879 and -122.4075** with your intersection's coordinates.

### How to find coordinates:
1. Go to Google Maps
2. Right-click exactly on your intersection
3. Click the lat/long numbers that appear at the top of the menu
4. They copy to your clipboard automatically

---

## Step 3 — Set the train direction

Find this section in index.html:

```html
<a-entity id="train-model-entity"
  gltf-model="#train-model"
  rotation="0 90 0"   ← CHANGE THIS
```

**The Y value (middle number) controls which direction the train faces:**

| Your street runs... | Set rotation Y to |
|--------------------|-------------------|
| East–West          | 90                |
| North–South        | 0                 |
| Northeast–Southwest | 45               |
| Northwest–Southeast | 315              |

Use Google Maps to draw a line along your street to see its bearing angle.

---

## Step 4 — Adjust scale & animation distance

**Scale** — if your train is too small or huge:
```html
<a-entity id="train-wrapper"
  scale="2.5 2.5 2.5"   ← change all three numbers equally
```
Start with 1 1 1 and go up. 2.5 works for most models sized as real-world meters.

**Travel distance** — how far the train travels before looping:
```html
<a-animation
  from="0 0 -30"   ← starts 30 metres behind anchor point
  to="0 0 30"      ← ends 30 metres ahead of anchor point
  dur="12000"      ← 12 seconds per pass (lower = faster)
```

---

## Step 5 — Deploy to a hosting service

**The AR experience MUST be served over HTTPS** — camera and GPS don't work on plain HTTP.

### Recommended free hosts:

**Netlify (easiest)**
1. Go to netlify.com → drag your entire folder onto the deploy area
2. You get a URL like `https://random-name.netlify.app`
3. Done. Share that URL as your QR code.

**GitHub Pages**
1. Create a repo, push all files
2. Settings → Pages → Source: main branch
3. URL: `https://yourusername.github.io/repo-name`

**Cloudflare Pages**
1. Connect your GitHub repo
2. Free, fast CDN, unlimited bandwidth

---

## Step 6 — Generate your QR code

1. Open `qr-generator.html` in your browser
2. Paste your hosted URL
3. Type your intersection label (e.g. "Main St & 1st Ave")
4. Click Generate
5. Click Print — use a weatherproof sign holder or laminate it

---

## Testing on your phone

Before going to the real location:

1. Host the files (Netlify drag-and-drop takes 30 seconds)
2. Open the URL on your phone
3. Tap "Board the Experience"
4. Grant camera + location permissions
5. Walk outside — the train should appear near your GPS coordinates

**Testing indoors / without GPS:**
- The train will still render, but may float or be misplaced until you're at the real location
- You can temporarily change the GPS coordinates to wherever you are right now for a quick test

---

## Smoke / chimney position

If smoke appears in the wrong spot relative to your locomotive:

```javascript
const chimneyOffset = { x: 0, y: 2.8, z: -1 };
//                         ↑       ↑      ↑
//                    left/right  height  front/back
```

Adjust `y` up or down to match your model's smokestack height.
Adjust `z` forward (negative) or back (positive) to align with the chimney.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Black screen / no camera | Must be HTTPS + grant camera permission |
| Train not visible | Walk to the GPS coordinates you set |
| Train too small | Increase scale values in train-wrapper |
| Train wrong direction | Adjust rotation Y value (see Step 3) |
| Model not loading | Check train.glb is in same folder as index.html |
| GPS shows wrong position | enableHighAccuracy is on — takes ~10s outdoors |
| Smoke in wrong place | Adjust chimneyOffset in the script block |

---

## Customizing the branding

The splash screen and HUD text are easy to change:

```html
<!-- Splash title -->
<h1 class="splash-title">The Ghost<br>Train</h1>

<!-- Splash year/subtitle -->
<p class="splash-year">Est. 1869 · AR Experience</p>

<!-- HUD location label -->
<p>Historic AR · Main &amp; 1st</p>
```

Colors are defined as CSS variables at the top of the `<style>` block:
```css
--rust:  #8b3a1a;   /* button background */
--ember: #c9541e;   /* accents */
--brass: #c8960c;   /* gold details */
--cream: #f5ead8;   /* text */
```

---

*Built with A-Frame 1.5 + AR.js 3.4.5 — no app download required.*
