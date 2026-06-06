# Custom components

You can add your own parts to the planner, and optionally contribute them to the
shared library so everyone gets them.

## Create a component in-app

1. In the Library pane, click **New custom component** (or **More ▸ Create
   custom component**).
2. Fill in what you know. Only **Name** is required; everything else is
   optional but makes the totals, filters, and health checks more useful:
   - **Category** decides the icon and how some checks treat the part.
   - **Voltage min/max**, **current draw**, **weight**, **price** feed the
     filters and the live BOM totals.
   - **System** (`analog`/`dji`/`hdzero`/`walksnail`) drives the system filter.
   - **Image** is stored inline for the library thumbnail.
   - **DXF** stores the filename only as a reference (see below).
3. Add the **pads** the part exposes — name, type, and an optional side
   (left/right/top/bottom). The pad **type** sets its colour and the wire type
   that's chosen automatically when you connect it. See the type list in
   [json-format.md](json-format.md#pin-object).
4. **Save**. The part appears in the Library with a *custom* tag and is stored
   in your browser. Use the pencil/✕ buttons on its card to edit or delete it.

Custom parts are included in **Export ▸ Project (.json)**, so they travel with
your workspace.

## A note on DXF footprints

The in-app editor stores only the **filename** of a DXF, as a reference — it
doesn't embed the geometry. To actually ship a footprint with a contributed
component, commit the `.dxf` under [`assets/`](../assets) and point the
component's `dxf` field at its path (e.g. `"dxf": "assets/my-board.dxf"`).

## Submit a component to the shared library

The public library is the [`/components`](../components) folder. To add a part:

1. In the component editor, click **Save & share…** — the app generates a clean
   JSON object and step-by-step instructions.
2. **Download** (or copy) the JSON.
3. Fork this repository and add the file as `components/<your-part-id>.json`.
4. Add `"components/<your-part-id>.json"` to the array in
   [`components/index.json`](../components/index.json).
5. If you have an image or DXF, commit them under `assets/` and reference them
   by path in the component.
6. Open a pull request. Once it's merged, the part loads for everyone using the
   deployed app.

Please keep one component per file, use a unique kebab-case `id`, and fill in as
many fields as you reasonably can. See [CONTRIBUTING.md](../CONTRIBUTING.md) for
the full checklist and the existing files in `/components` for examples.
