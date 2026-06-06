# JSON formats

Everything the FPV Build Planner exports is plain, human-readable JSON that you
can hand-edit, diff, and commit. This document is the reference for each shape.

All exports include a `schema` string of the form `fpv-build-planner/<kind>@<version>`
so tools can tell them apart.

---

## Component

A component is one part in the library. Built-in parts live inside the app; the
public library is the [`/components`](../components) folder, and you can also
create components in-app (stored in your browser).

```json
{
  "id": "speedybee-f405-v4-fc",
  "name": "SpeedyBee F405 V4",
  "brand": "SpeedyBee",
  "model": "F405 V4",
  "category": "fc",
  "description": "30.5mm F405 flight controller with Bluetooth config.",
  "voltageMin": 7.4,
  "voltageMax": 30.0,
  "currentDraw": 0.2,
  "width": 36,
  "height": 36,
  "weight": 8.5,
  "mountSpacing": "30.5",
  "protocol": "analog",
  "price": 35,
  "cellMin": 2,
  "cellMax": 6,
  "datasheet": "https://example.com/datasheet",
  "notes": "Free-text notes shown in the inspector.",
  "dxf": "assets/speedybee-f405-v4.dxf",
  "pins": [
    { "name": "VBAT", "type": "vbat", "side": "l" },
    { "name": "GND",  "type": "gnd",  "side": "l" },
    { "name": "TX1",  "type": "tx",   "side": "r" }
  ]
}
```

### Component fields

| Field | Type | Required | Notes |
|---|---|---|---|
| `id` | string | yes | Unique, kebab-case. Matching an existing id overrides that part. |
| `name` | string | yes | Display name. |
| `category` | string | yes | One of the categories below. |
| `brand`, `model` | string | no | Shown under the name. |
| `description`, `notes` | string | no | Free text. |
| `voltageMin`, `voltageMax` | number | no | Operating voltage range (V). Powers the voltage filter and health checks. |
| `currentDraw` | number | no | Typical current (A). Feeds the current estimate. |
| `weight` | number | no | Grams. Feeds the weight total. |
| `price` | number | no | USD. Feeds the cost total. |
| `width`, `height` | number | no | Millimetres. |
| `mountSpacing` | string | no | e.g. `"30.5"`, `"20"`, `"16/20"`. |
| `protocol` | string | no | `analog` \| `dji` \| `hdzero` \| `walksnail` — drives the system filter. |
| `cellMin`, `cellMax` | number | no | LiPo cell range (S). |
| `datasheet` | string (URL) | no | Linked in the inspector. |
| `dxf` | string (path) | no | Reference to a DXF footprint committed under `assets/`. |
| `pins` | array | yes | The pads/pins; see below. |

### Categories

`fc`, `esc`, `vtx`, `camera`, `rx`, `gps`, `motor`, `battery`, `cap`, `buzzer`,
`led`, `vrx`, `other`.

### Pin object

```json
{ "name": "VBAT", "type": "vbat", "side": "l" }
```

- `name` — label drawn on the pad.
- `type` — drives the pad colour, the auto wire type, and the health checks.
  One of: `vbat`, `gnd`, `5v`, `9v`, `3v3`, `tx`, `rx`, `sbus`, `cam`, `vtx`,
  `video`, `led`, `motor`, `signal`, `custom`.
- `side` — optional placement hint: `l` (left), `r` (right), `t` (top),
  `b` (bottom). Omit to let the app place it automatically.

---

## components/index.json

The library manifest is an array. Entries are **relative paths** to component
files (recommended) and/or **inline component objects**.

```json
[
  "components/speedybee-f405-v4-fc.json",
  "components/tmotor-f55a-pro-ii-esc.json",
  { "id": "inline-part", "name": "Inline Part", "category": "other", "pins": [] }
]
```

When the app is deployed it fetches this file and merges the results into the
built-in library. Missing or unreachable entries are skipped, so the app still
works offline / on `file://`.

---

## Build

A build is one drone: its specs, its parts list (the authoritative BOM), and its
wiring diagram.

```json
{
  "id": "example-analog-5in",
  "name": "Analog 5\" Freestyle",
  "frameSize": "5\"",
  "propSize": "5x4.3x3",
  "cells": 6,
  "motorKV": 1900,
  "notes": "Free text.",
  "components": [
    { "compId": "fc-generic-f7", "qty": 1 },
    { "compId": "motor-2207", "qty": 4 }
  ],
  "diagram": {
    "nodes": [
      {
        "id": "n1",
        "compId": "fc-generic-f7",
        "name": "F7 Flight Controller",
        "x": 540, "y": 120,
        "locked": false,
        "pins": [
          { "id": "n1p1", "name": "VBAT", "type": "vbat", "side": "l" }
        ]
      }
    ],
    "wires": [
      {
        "id": "w1",
        "from": { "node": "n1", "pin": "n1p1" },
        "to":   { "node": "n2", "pin": "n2p1" },
        "type": "power",
        "color": "#ff4d4f",
        "label": "",
        "style": "curved"
      }
    ]
  },
  "createdAt": 0,
  "updatedAt": 0
}
```

Notes:

- `components[].compId` references a component `id`. `qty` is the BOM quantity.
- Each node carries its **own copy** of the component's pins (with per-node pin
  `id`s), so a build stays valid even if the source component later changes or
  isn't installed.
- A wire's `from`/`to` reference a node `id` and a pin `id` within that node.
- `type` is one of the wire types below; `color` is any CSS colour; `style` is
  `curved` or `straight`.

### Wire types & default colours

| type | colour | typical use |
|---|---|---|
| `power` | `#ff4d4f` | VBAT |
| `ground` | `#9aa6b5` | GND |
| `5v` | `#ff9f0a` | 5V rail |
| `9v` | `#ff7a00` | 9V/10V (VTX) rail |
| `signal` | `#ffd60a` | ESC signal, generic |
| `video` | `#38d66a` | analog video |
| `uart` | `#3a9bff` | TX/RX serial pairs |
| `motor` | `#c06bff` | motor phases |
| `custom` | `#19e3c4` | anything else |

---

## Project (full export / import)

`Export ▸ Project (.json)` writes every build plus your custom components and
settings. Importing the same file restores them (colliding build ids are
regenerated so nothing is overwritten).

```json
{
  "schema": "fpv-build-planner/project@1",
  "exportedAt": "2026-01-01T00:00:00.000Z",
  "builds": [ /* array of Build objects */ ],
  "customComponents": [ /* array of Component objects */ ],
  "settings": { "route": "curved", "grid": false, "labels": true, "theme": "dark" }
}
```

The importer also accepts a **single build object** (with a `diagram`) or a
**bare diagram** (`{ "nodes": [...], "wires": [...] }`).

See [`examples/analog-5inch-freestyle.json`](../examples/analog-5inch-freestyle.json)
for a complete, loadable project.

---

## BOM export

`Export ▸ BOM as JSON`:

```json
{
  "schema": "fpv-build-planner/bom@1",
  "build": "Analog 5\" Freestyle",
  "items": [
    {
      "name": "Motor 2207",
      "brand": "Generic",
      "model": "2207 1900KV",
      "category": "Motor",
      "qty": 4,
      "unitWeight": 31,
      "unitPrice": 18,
      "unitCurrent": 30
    }
  ],
  "totals": { "parts": 11, "weight": 168, "cost": 214, "current": 120 }
}
```

`Export ▸ BOM as CSV` writes the same data as a spreadsheet-friendly file with a
`TOTAL` row.

---

## Theme export

`More ▸ Export theme` captures the current palette so you can reuse or fork it.

```json
{
  "schema": "fpv-build-planner/theme@1",
  "theme": "dark",
  "cssVariables": { "--accent": "#19e3c4", "--bg": "#0c0f14" },
  "wireColors": { "power": "#ff4d4f", "ground": "#9aa6b5" }
}
```

---

## Share links

`Share` copies a URL with a compact, URL-safe base64 payload in the hash
(`#b=...`) describing the current build (name, specs, parts, diagram). Opening
the link creates a fresh copy of that build. Links are self-contained — no
server is involved — so very large diagrams make very long URLs.
