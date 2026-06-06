<!-- Thanks for contributing! Fill in what's relevant and delete the rest. -->

## What does this PR do?

Briefly describe the change.

## Type of change

- [ ] New component(s) for the library
- [ ] Fix to an existing component
- [ ] Bug fix (app)
- [ ] New feature (app)
- [ ] Docs
- [ ] Other:

---

## Adding/updating components

If this PR touches `/components`, please confirm:

- [ ] One component per file, unique kebab-case `id`.
- [ ] Each file is registered in `components/index.json`.
- [ ] `name`, `category`, and a `pins` array are present; `category` and every
      pin `type` are valid (see `docs/json-format.md`).
- [ ] Specs filled in where known (voltage, weight, current, price, dimensions,
      cell range).
- [ ] Any images/DXF are committed under `assets/` and referenced by path.
- [ ] JSON validates (`python3 -m json.tool components/<id>.json`).
- [ ] I have the right to share any images included.

## Code changes

If this PR changes `index.html`:

- [ ] No new external/runtime dependencies; still works from `file://` and offline.
- [ ] `node --check` passes on the extracted `<script>` (see CONTRIBUTING.md).
- [ ] Verified in the browser; both dark and light themes look right.

## Screenshots (if UI changes)

## Related issues

Closes #
