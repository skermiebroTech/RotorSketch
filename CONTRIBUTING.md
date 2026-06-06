# Contributing

Thanks for helping make the FPV Build Planner better! The single most useful
contribution is **adding components to the library**, but bug reports, fixes,
and feature ideas are all welcome.

## Adding a component (the common case)

The library lives in [`/components`](components). Each part is one JSON file,
listed in [`components/index.json`](components/index.json).

1. **Generate the JSON.** The easiest path: open the app, create the part with
   **New custom component**, then click **Save & share…** to get a clean JSON
   object and instructions. You can also copy an existing file in `/components`
   and edit it.
2. **Add the file** as `components/<id>.json`, using a unique, kebab-case `id`
   (e.g. `tmotor-f55a-pro-ii-esc.json`).
3. **Register it** by adding `"components/<id>.json"` to the array in
   `components/index.json`.
4. **Assets (optional).** Commit any image or `.dxf` under [`assets/`](assets)
   and reference them by path (`"dxf": "assets/<id>.dxf"`).
5. **Open a pull request.**

### Component checklist

- [ ] Unique, kebab-case `id`; one component per file.
- [ ] `name`, `category`, and a `pins` array are present.
- [ ] `category` is one of the valid categories (see
      [docs/json-format.md](docs/json-format.md#categories)).
- [ ] Each pin has a valid `type` (see
      [docs/json-format.md](docs/json-format.md#pin-object)).
- [ ] Realistic specs filled in where known (voltage range, weight, current,
      price, dimensions, cell range).
- [ ] Registered in `components/index.json`.
- [ ] The JSON is valid (`python3 -m json.tool components/<id>.json`).
- [ ] No copyrighted images committed without the right to share them.

See the full reference in [docs/json-format.md](docs/json-format.md).

## Code contributions

The app is a single, dependency-free [`index.html`](index.html): HTML + inline
CSS + a single IIFE of namespaced modules (`App.Config`, `App.State`,
`App.Editor`, `App.Build`, `App.BOM`, `App.Validate`, `App.Presets`, `App.UI`,
plus the boot block). There is **no build step**.

Guidelines:

- Keep it dependency-free and self-contained — no bundlers, no external runtime
  libraries. Everything must keep working from `file://` and offline.
- Match the existing style: namespaced modules on the global `App` object,
  small focused functions, CSS variables for all colours so both themes work.
- Don't introduce `localStorage` keys beyond the existing one without good
  reason; respect the "no data leaves the browser" property.
- If you add a feature with new UI, wire it through `App.UI` and make sure
  `fullRefresh()` / `afterChange()` keep the views in sync.

### Sanity-checking your change

There's no test runner, but please at least:

```bash
# 1) the JS still parses — extract the <script> and check it
python3 - <<'PY'
import re; html=open('index.html',encoding='utf-8').read()
open('/tmp/app.js','w').write(re.search(r'<script>(.*)</script>',html,re.S).group(1))
PY
node --check /tmp/app.js

# 2) all JSON is valid
find components builds examples -name '*.json' -print -exec python3 -m json.tool {} \; >/dev/null

# 3) click through the app from a local server
python3 -m http.server 8000
```

Then verify the relevant tab (Wiring / Build & BOM / Health) behaves, and that
both dark and light themes look right.

## Reporting bugs & requesting features

Use the issue templates:

- **Bug report** — what you did, what happened, what you expected, browser/OS.
- **Component request** — the part and a datasheet link.
- **Feature request** — the problem you're trying to solve.

## Code of conduct

Be kind and constructive. Assume good faith. Keep discussion focused on the
project.
