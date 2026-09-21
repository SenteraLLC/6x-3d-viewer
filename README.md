# Sentera 6X Interactive 3D Viewer

Public static site (GitHub Pages) hosting interactive `<model-viewer>`-based
glTF viewers embedded into the [gitbook-6x-user-guide](https://github.com/SenteraLLC/gitbook-6x-user-guide)
support docs via `{% embed %}` / iframe.

The GLB models here are **web-optimized re-tessellations** (Draco-compressed,
mesh-simplified) generated from internal CAD/STEP source files. No STEP/CAD
source, assembly metadata, or proprietary design data is published here —
only the visual mesh geometry needed for the interactive viewer.

## Structure

```
index.html              landing page
viewer.html             the embeddable viewer (?model=<key>)
models/*.glb            optimized glTF binaries
hotspots/*.json         hotspot definitions per model
```

## Adding / moving a hotspot

1. Open `viewer.html?model=<key>&` in a browser (or via the deployed Pages URL).
2. Click **Edit Hotspots**, then click anywhere on the model.
3. A JSON snippet with the exact `position`/`normal` under your cursor appears
   in the bottom panel — copy it.
4. Edit the corresponding `hotspots/hotspots-*.json` file: update the matching
   entry's `position`/`normal`, or add a new entry with a unique `id`.
5. Commit and push. GitHub Pages redeploys automatically within a minute.

## Adding a new model

1. Convert STEP → GLB (Draco-compressed) and drop the `.glb` into `models/`.
2. Add a `hotspots/hotspots-<key>.json` file (array of `{id, label,
   description, position, normal}`).
3. Register the model in the `MODELS` map in `viewer.html`.
4. Link to it as `viewer.html?model=<key>` from the docs.
