<div align="center">

# FPV Build Planner

**Plan FPV drone builds end-to-end — wire it on a node canvas, track the parts
list, sanity-check the wiring, and export everything.**

No account. No backend. Runs offline. One HTML file.

</div>

---

## What it does

- **Node-based wiring canvas** — drop components, then drag pad-to-pad to draw
  wires. Curved or straight routing, snap-to-grid, zoom/pan, undo/redo,
  duplicate, lock, labels, and per-wire colours/types. It's all SVG, so it
  exports cleanly.
- **Component library** — built-in parts (FCs, ESCs, VTXs, cameras, RX, GPS,
  motors, batteries, caps, buzzers, LEDs, VRX) plus your own custom parts. When
  deployed, the app also loads the community library from [`/components`](components).
- **Build planner & BOM** — frame/prop/cell/KV/notes, an authoritative parts
  list with live **weight, cost, and peak-current** estimates, and a **UART
  planner** that reads your wiring. Export the BOM as **CSV** or **JSON**.
- **Build health check** — scans for the usual FPV mistakes (shorts, a 5V pad on
  VBAT, missing grounds, crossed receiver serial, UART conflicts, missing ESC
  signals, over-voltage parts, no cap on a high-power build) and scores the
  build. Advisory only.
- **Presets** — ready-made, pre-wired builds: Analog 5", DJI O3, HDZero,
  Walksnail, Tinywhoop 1S, and Long-range GPS.
- **Exports** — wiring diagram as **SVG**/**PNG**, BOM as **CSV**/**JSON**, the
  whole workspace as a project **JSON**, and a **shareable link** with the build
  encoded in the URL.
- **Offline / PWA** — installable and fully usable without a connection once
  loaded.
- **Dark & light themes** — carbon-dark by default; export your palette as JSON.

> [!NOTE]
> Screenshots go here — drop a couple of PNGs in `assets/` and reference them,
> e.g. `![Wiring canvas](assets/screenshot-wiring.png)`.

## Try it

It's a single static file — open `index.html` in any modern browser and you're
running. Component-library fetching and offline caching only kick in when it's
served over `http(s)` (i.e. deployed or via a local server), but every other
feature works straight from the file.

### Run locally

```bash
# from the repo root
python3 -m http.server 8000
# then open http://localhost:8000
```

(or any static server — `npx serve`, `php -S localhost:8000`, etc.)

## Deploy to GitHub Pages

This repo ships a workflow at
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) that publishes the
whole repository to GitHub Pages.

1. Push the repo to GitHub.
2. **Settings ▸ Pages ▸ Build and deployment ▸ Source** → **GitHub Actions**.
3. Push to `main` (or run the workflow manually). Your site appears at
   `https://<user>.github.io/<repo>/`.

Because everything is relative, it also works from a project subpath without any
configuration.

## Repository structure

```
.
├── index.html                  # the entire app (HTML + CSS + JS, no build step)
├── manifest.webmanifest        # PWA manifest
├── service-worker.js           # offline cache
├── assets/
│   └── favicon.svg
├── components/                 # the community component library
│   ├── index.json              # manifest: paths to component files
│   └── *.json                  # one part per file
├── builds/
│   └── *.build.json            # example single builds
├── examples/
│   └── *.json                  # complete, loadable project exports
├── docs/
│   ├── usage.md
│   ├── custom-components.md
│   └── json-format.md          # full schema reference
└── .github/                    # issue/PR templates + Pages workflow
```

## Design notes

- **One file on purpose.** The app is a single `index.html` with inline CSS and
  JS so it renders anywhere, has zero build step, and works straight from disk.
  The JavaScript is organised into namespaced modules (`App.Config`, `App.State`,
  `App.Editor`, `App.Build`, `App.BOM`, `App.Validate`, `App.Presets`, `App.UI`,
  …) inside a single IIFE.
- **Progressive enhancement.** The library is embedded *and* fetched from
  [`/components`](components) at runtime; if the fetch fails (offline, `file://`),
  the embedded set is used. Same idea for examples.
- **Your data is yours.** State lives in `localStorage`; there is no server and
  nothing is transmitted. Exports and share links are self-contained.

## Contributing

Adding your gear to the library is the most useful contribution — see
[CONTRIBUTING.md](CONTRIBUTING.md) and
[docs/custom-components.md](docs/custom-components.md). Bug reports and feature
ideas are welcome via the issue templates.

## Disclaimer

This is a **planning aid**, not an electrical authority. The health checks are
heuristics and won't catch everything. Always double-check against your specific
hardware's documentation before soldering or powering anything.

## License

Released under the [Apache License](LICENSE).
