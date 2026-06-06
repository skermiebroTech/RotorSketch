# Usage guide

A walk-through of the FPV Build Planner. Nothing here is destructive — your work
auto-saves to your browser, and everything can be exported to plain JSON.

## The layout

- **Left** — the Library (parts you can add), your Builds, and Presets.
- **Centre** — four tabs: **Wiring**, **Build & BOM**, **Health**, **Docs**.
- **Right** — the Inspector. Select a node or wire to edit it here.

## Wiring canvas

1. **Add a part** — click a component in the Library. It drops on the canvas
   *and* is added to the parts list. You can also drag a component onto the
   canvas.
2. **Draw a wire** — drag from one pad (the little ring on a node) to another
   pad. The nearest pad highlights as you hover; release to connect. The wire
   type and colour are chosen automatically from the two pads (e.g. VBAT→VBAT is
   a red power wire, TX↔RX is a blue UART wire).
3. **Move / pan / zoom**
   - Move a node: drag its body.
   - Pan: drag empty space, hold <kbd>Space</kbd> and drag, or use the middle
     mouse button.
   - Zoom: scroll wheel, or the toolbar `+` / `−` / fit buttons.
4. **Edit a wire** — single-click it, then use the Inspector to change its type,
   colour, label, or routing, or delete it. Double-click a wire to flip just
   that wire between curved and straight.
5. **Toolbar**
   - **Curved / Straight** — default routing for new wires.
   - **Snap** — snap nodes to the grid while dragging.
   - **Labels** — show/hide wire labels.
   - **Arrange** — auto-layout the nodes by role (power → FC → peripherals).
   - **Clear** — remove everything from the canvas (asks first).

### Keyboard shortcuts

| Key | Action |
|---|---|
| <kbd>Ctrl/⌘</kbd>+<kbd>Z</kbd> | Undo |
| <kbd>Ctrl/⌘</kbd>+<kbd>Y</kbd> / <kbd>Ctrl/⌘</kbd>+<kbd>Shift</kbd>+<kbd>Z</kbd> | Redo |
| <kbd>Delete</kbd> / <kbd>Backspace</kbd> | Remove the selection |
| <kbd>F</kbd> | Fit the diagram to the view |
| <kbd>Ctrl/⌘</kbd>+<kbd>S</kbd> | Export the project as JSON |
| <kbd>Esc</kbd> | Close a dialog |

## Build & BOM

The **Build & BOM** tab holds:

- **Specs** — frame size, prop, cell count, motor KV, and notes.
- **Live totals** — all-up weight, cost, and a rough peak-current estimate,
  plus a battery sanity pill from your cell count.
- **Parts list** — the authoritative bill of materials. Adjust quantities with
  the `+ / −` controls, or remove a row. Export it as **CSV** or **JSON**.
- **UART planner** — reads your wiring and shows which flight-controller serial
  ports are in use (and by what) and which are free.

> Adding a component from the Library increases the matching parts-list
> quantity. Deleting a node from the canvas does **not** decrease the count —
> the parts list is yours to manage directly.

## Health check

The **Health** tab scans the diagram for common FPV mistakes and shows a score
with a colour-coded ring. Checks include:

- power/ground shorts, and a 5V pad wired to VBAT,
- devices missing a ground,
- ESC signal wires and the FC↔ESC link,
- the analog video path, and a reminder for digital VTX control over UART,
- a receiver with its serial pair not crossed,
- two devices fighting over the same UART,
- battery over-voltage and pack/cell mismatches,
- no capacitor on a high-power build.

Everything is **advisory** — the planner never stops you building what you want.

## Presets

The **Presets** tab has ready-made, pre-wired builds (analog, DJI O3, HDZero,
Walksnail, tinywhoop, long-range GPS). Loading one fills the current build; if
the current build already has wiring you'll be asked first.

## Saving, sharing & offline

- **Auto-save** — your work is saved to this browser automatically.
- **Export ▸ Project (.json)** — the whole workspace (all builds + custom parts
  + settings). Re-import it anywhere with **Export ▸ Import project**.
- **Share** — copies a URL with the current build encoded in it; open it
  elsewhere to load a copy.
- **Offline** — once loaded, the app is installable and runs without a network
  connection.

## Custom components

See [custom-components.md](custom-components.md) for creating your own parts and
submitting them to the shared library.
