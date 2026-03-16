# 🌌 Nebula Genesis — N-Body Gravity Simulator

This was a big project that took me a while, you can see it **[here](https://its-idhem.github.io/Nebula-Genesis/)**. 
An interactive, browser-based N-body gravity simulator that models the full lifecycle of a stellar system — from a primordial molecular cloud to planetary formation, stellar evolution, and eventual stellar death.

![Phase I: Giant Molecular Cloud](https://img.shields.io/badge/Phase%20I-Molecular%20Cloud-cc88ff?style=flat-square) ![Phase V: Late Heavy Bombardment](https://img.shields.io/badge/Phase%20V-Late%20Heavy%20Bombardment-ff8844?style=flat-square) ![Stars](https://img.shields.io/badge/Stars-9%20spectral%20types-ffe87c?style=flat-square) ![Remnants](https://img.shields.io/badge/Remnants-WD%20%7C%20NS%20%7C%20BH-a855f7?style=flat-square)

---

## ✨ Features

### Physics Engine
- **Barnes-Hut tree** (θ = 0.6) for O(n log n) gravity — handles hundreds of bodies in real time
- **Adaptive sub-stepping** — bodies under high acceleration get up to 3× more integration steps per tick for stability
- **Dynamic gravitational softening** — scales with body radius to prevent close-encounter blow-ups
- **Spatial hash collision detection** — O(1) average collision lookup
- **Tidal disruption** — small bodies passing within the Roche limit of a massive object are shredded into debris streams (active from Phase III onward)
- **Size-dependent gas drag** — smaller particles circularise faster (Epstein drag regime), larger planetesimals take longer
- **Angular momentum conserving mergers** — colliding bodies exchange momentum correctly
- **Velocity cap** — prevents numerical blow-ups during close encounters
- **Hard body cap (1800)** — lowest-mass bodies are culled if count spikes, keeping the sim stable

### Stellar Lifecycle (5 Phases)

| Phase | Age | Event |
|-------|-----|-------|
| **I** | 0–0.5 Gyr | Giant Molecular Cloud — H₂/He gas with solid-body rotation and turbulence |
| **II** | 0.5–0.9 Gyr | Jeans instability — gradual protostellar collapse, disk flattening |
| **III** | 2.0 Gyr | Protoplanetary disk — gas drag circularises orbits, dust grains stick |
| **IV** | 4.0 Gyr | T Tauri stellar winds — visible expanding shockwave clears residual gas |
| **V** | 5.5 Gyr | Late Heavy Bombardment — asteroid belt impactors with exponential decay rate |

### Stellar Evolution
- **9 spectral types**: Hypergiant, Blue Giant, B, A, F, G, K, M, and transitional Red Giant / Aging Star
- **Gradual red giant swell** — stars expand over the final 15% of their main-sequence lifetime, with mass-dependent swell factors (hypergiants swell far more than dwarfs)
- **Stellar death** triggered at end of lifetime:
  - Low/mid-mass stars → Planetary Nebula → **White Dwarf**
  - High-mass stars → **Supernova** → Neutron Star
  - Very massive stars → Core Collapse → **Black Hole**
- **White Dwarf → Black Dwarf** cooling over ~15 Gyr
- **Chandrasekhar limit** — white dwarfs accreting past 1.4 M☉ trigger a **Type Ia supernova**, completely destroying themselves
- **Tolman–Oppenheimer–Volkoff limit** — neutron stars accreting past ~2.1 M☉ collapse to black holes
- **Radiation pressure stellar winds** — massive stars slowly lose mass throughout their lives (Reimers/de Jager prescription)
- **Compressed timescales** for playability — stellar deaths are observable within a single session

### Sub-Stellar & Planetary Objects
- Gas Giant, Brown Dwarf, Ice Giant, Rocky Planet, Debris/Asteroid
- Classification based on mass thresholds, with correct ordering (Gas Giant → Brown Dwarf → Ice Giant)
- Fragmentation on high-speed similar-mass collisions (Phase III+)

### Object Inspector
Click any body to select and follow it. The inspector panel shows:
- Classification, mass (M☉ or M⊕), radius (AU or km), speed (km/s)
- Temperature and luminosity estimates
- Angular momentum and kinetic energy
- **Orbital elements** — semi-major axis (AU), eccentricity (4 d.p.), orbital period (yr/kyr), or "hyperbolic" for unbound trajectories
- **Surface gravity** (sim units)
- **Lifecycle status** — remaining main-sequence life, red giant percentage, white dwarf cooling countdown, neutron star % of TOV limit, black hole Schwarzschild radius estimate

### Object Search
Click the 🔍 button (top-right, left of the legend) to open the search panel:
- Browse all active object classifications with live body counts
- Click a category to see every body of that type, sorted by mass
- Click any body to instantly select it, close the panel, and snap the camera to follow it

---

## 🎮 Controls

### Mouse
| Action | Result |
|--------|--------|
| Click a body | Select & follow it |
| Click empty space | Deselect |
| Drag | Pan camera (disengages follow) |
| Scroll wheel | Zoom toward cursor |

### Keyboard
| Key | Action |
|-----|--------|
| `+` / `-` | Zoom in / out toward canvas centre |
| `W` `A` `S` `D` | Pan camera (disengages follow) |
| `↑` `↓` `←` `→` | Pan camera |

### Toolbar (bottom)
| Button | Function |
|--------|----------|
| **PAUSE / RESUME** | Freeze simulation |
| **Time slider** | 0.1× – 8× time dilation |
| **GRID** | Toggle spacetime curvature grid |
| **ORBITS** | Toggle orbit trails |
| **CENTRE ★** | Lock camera to most massive body |
| **Deselect** | Unfollow selected body |
| **☰ Menu** | Return to start screen and pick particle count |

---

## 🔭 Object Classification

### Stellar
| Type | Colour |
|------|--------|
| Hypergiant / Wolf-Rayet | Bright blue `#1a8fff` |
| Blue Giant (O-type) | Blue `#6ab4ff` |
| B-type Star | Light blue `#b0d8ff` |
| A-type Star | White `#fff4ea` |
| F-type Star | Yellow-white `#fff2c0` |
| G-type / Sun-like | Yellow `#ffe87c` |
| K-type Star | Orange `#ffa040` |
| M-type Red Dwarf | Red `#ff4422` |
| Red Giant / Aging Star | Deep red `#ff2200` |

### Stellar Remnants
| Type | Colour |
|------|--------|
| White Dwarf | White `#ffffff` |
| Black Dwarf | Near-invisible dark `#111827` |
| Neutron Star | Ice blue `#88ccff` |
| Black Hole | Black with purple glow `#a855f7` |

### Sub-Stellar / Planetary
Gas Giant → Brown Dwarf → Ice Giant → Rocky Planet → Debris

---

### Key Physics Notes
- **Gas particles** are non-gravitational in Phase I (soft blobs). They transition to gravitational point masses *gradually* across 0.5–0.9 Gyr to prevent a collision cascade.
- **Fragmentation** is disabled during Phase I/II — gas cloud collisions always merge.
- **Tidal disruption** is only active from Phase III onward.
- **pendingBodies[]** staging array prevents mutating the bodies array mid-iteration (which caused crashes in earlier versions).
- Stellar lifetimes are compressed ~20× from real astrophysical values so deaths are observable in a normal session.

The simulation runs entirely client-side. An internet connection is only needed to load Tailwind CSS and the Rajdhani / Share Tech Mono fonts from CDN (or use the GitHub Pages hosted version).

---

## 🌠 Tips for Best Experience

- Start with **900–1200 particles** for a good balance of detail and performance
- Use **CENTRE ★** to lock onto the growing protostar during Phase II
- Watch for the **T Tauri wind front** expanding outward in Phase IV — a visible amber shockwave
- After ~5 Gyr, use the **search panel** to find any Red Giants and select them — you can watch them swell and eventually explode
- At high time dilation (6–8×), stellar deaths happen fast — keep an eye on the toast notifications
- If a **White Dwarf** accretes enough mass, it will detonate as a **Type Ia supernova** with no remnant — the most violent event in the sim

---

## 📐 Scientific Concepts Modelled

| Concept | Implementation |
|---------|---------------|
| Jeans instability | Phase II trigger at 0.5 Gyr |
| Bondi-Hoyle accretion | Protostar mass growth rate scales with current mass |
| Epstein drag regime | Gas drag strength inversely proportional to particle size |
| Roche limit | Tidal disruption at `d < 2.44 · R_big · (M_big/M_small)^(1/3)` |
| Chandrasekhar limit | White dwarf Type Ia at 1.4 M☉ |
| TOV limit | Neutron star collapse to BH at ~2.1 M☉ |
| Reimers wind | Radiation pressure mass loss for massive stars |
| Late Heavy Bombardment | Exponentially decaying impactor rate from belt zone |
| Vis-viva equation | Orbital eccentricity computed from energy and angular momentum |
| Kepler's 3rd law | Orbital period from semi-major axis |

---

## 🙏 Built With

- **Vanilla JavaScript** — no frameworks
- **Canvas 2D API** — all rendering
- [Tailwind CSS](https://tailwindcss.com/) via CDN — UI styling
- [Rajdhani](https://fonts.google.com/specimen/Rajdhani) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) — typography
- A bit of AI for the maths, and optimisations (extremely helpful)
