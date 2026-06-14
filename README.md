# 🌍 Live World Clock Wallpaper

A fully self-contained HTML5 desktop wallpaper featuring a real-time world map with per-pixel day/night/evening/morning coloring, 176 city lights that activate strictly based on sun altitude, a seven-segment LED clock, and 12 live timezone cards — all in a single file with zero backend and zero build step.

> **Vibe-coded with [Claude](https://claude.ai) in ~2 hours.**

---

## 📽️ Demo

https://github.com/user-attachments/assets/demo.mp4

> *Map colors shift in real time as Earth rotates. City lights only glow on the night side. The LED clock at the top automatically uses your local timezone.*

---

## ✨ Features

- **Real-time world map** — Natural Earth 110m TopoJSON rendered via D3.js `geoNaturalEarth1` projection
- **Per-pixel sky state coloring** — every land pixel is individually colored based on solar position:
  - 🟢 **Daytime** — sun > 30° above horizon (bright green)
  - 🟠 **Evening** — sun 0–30°, descending (orange → rust)
  - 🟡 **Morning** — sun 0–30°, ascending (amber → gold)
  - 🔵 **Night** — sun below −14° (deep navy)
- **176 city lights** — radial gradient glows that only activate when the sun is below the horizon at that city's exact coordinates. Zero bleed into daytime areas.
- **Seven-segment LED display** — custom canvas-drawn digit renderer that changes color based on local sky state (green by day, orange at evening, amber at morning, blue at night)
- **Auto timezone detection** — reads your OS timezone via `Intl.DateTimeFormat().resolvedOptions().timeZone` — no configuration needed. Shows your city name and UTC offset automatically.
- **12 live timezone clocks** — cards color-coded by sky state, updating every second
- **Blinking colon** — authentic LED clock behavior
- **Sub-solar point badge** — live lat/lon of the point on Earth directly under the sun

---

## 🖥️ Setting as Live Wallpaper on Windows

### Step 1 — Install Lively Wallpaper

Download and install **[Lively Wallpaper](https://apps.microsoft.com/store/detail/lively-wallpaper/9NTM2QC6QWS7)** from the Microsoft Store — it is completely free.

Or paste this in your browser address bar to open the Store page directly:
```
ms-windows-store://pdp/?productid=9ntm2qc6qws7
```

### Step 2 — Download the wallpaper file

Download `world-clock-wallpaper.html` from this repo and save it somewhere permanent — for example:
```
C:\Users\YourName\Wallpapers\world-clock-wallpaper.html
```

### Step 3 — Add to Lively

1. Open **Lively Wallpaper** from the Start menu
2. Click the **+** button (top right)
3. Click **Browse** → navigate to `world-clock-wallpaper.html` → **Open**
4. Give it a name → click **OK**
5. It immediately becomes your live wallpaper ✅

### Step 4 — Recommended Settings

| Setting | Value | Reason |
|---|---|---|
| FPS cap | 10 | Clock only needs 1fps, 10 is generous and saves CPU |
| Pause on battery | On | Saves power on laptops |
| Pause when fullscreen | On | No interference while gaming or watching video |

> **Why Lively?** Lively uses Chromium Embedded Framework (CEF) — the full JavaScript engine runs behind your windows. The wallpaper is not a screenshot — clocks tick every second and the map redraws every 10 seconds in real time.

---

## 🧠 How It Works

### Solar Position Math

The sky state of every land pixel is computed from scratch every 10 seconds using real astronomy:

```
UTC time
  → day of year
  → solar declination:      dec  = 23.45° × sin(B)
  → equation of time:       eot  = 9.87sin(2B) − 7.53cos(B) − 1.5sin(B)
  → sub-solar longitude:    sLon = −(UTC_hours − 12) × 15 + eot/4
  → sun altitude at (lon, lat):
      alt = arcsin( sin(φ)sin(δ) + cos(φ)cos(δ)cos(H) )
  → hour angle:             H = lon − sLon
      H > 0  →  sun past meridian  →  evening
      H < 0  →  sun before meridian  →  morning
```

### Sky State Thresholds

| State | Condition | Map Color | Card Color | LED Color |
|---|---|---|---|---|
| ☀️ Day | alt > 30° | Green | Green | Green |
| 🌇 Evening | 0° < alt ≤ 30°, H > 0 | Orange | Orange | Orange |
| 🌄 Morning | 0° < alt ≤ 30°, H < 0 | Amber | Amber | Amber |
| 🌙 Night | alt < −14° | Dark navy | Blue | Blue |

All three layers — map, clock cards, and LED display — use the same `skyState()` function and flip at the exact same moment.

---

## 🔧 Tech Stack

| Technology | Used For |
|---|---|
| **HTML5 Canvas API** | 4 stacked canvases: stars · map · night overlay · city lights |
| **D3.js v7** | `geoNaturalEarth1` projection · `geoPath` · `projection.invert()` for per-pixel lon/lat |
| **TopoJSON + Natural Earth 110m** | Real country polygon data decoded at runtime |
| **Solar Position Math** | Custom vanilla JS — declination, equation of time, hour angle, sun altitude |
| **Intl.DateTimeFormat** | Browser-native timezone conversion — zero external libraries |
| **CSS** | `position:absolute` layout · `clamp()` for responsive sizing · state transitions |
| **Google Fonts** | Orbitron (clocks + LED) · Share Tech Mono (labels) |

---

## 📁 Repository Structure

```
world-clock-wallpaper.html   ← the entire project, single file
demo.mp4                     ← screen recording of the live wallpaper
README.md                    ← this file
```

---

## ⚡ Requirements

- Windows 10 or 11
- [Lively Wallpaper](https://apps.microsoft.com/store/detail/lively-wallpaper/9NTM2QC6QWS7) (free, Microsoft Store)
- Internet connection on **first load only** — fetches D3.js and TopoJSON from CDN once, then works fully offline

---

## 📄 License

MIT — do whatever you want with it.

---

*Built with Claude Sonnet · April 2026*
