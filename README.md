# Sentera 6X Interactive 3D Viewer

Public static site (GitHub Pages) hosting interactive `<model-viewer>`-based
glTF viewers embedded into the [gitbook-6x-user-guide](https://github.com/SenteraLLC/gitbook-6x-user-guide)
support docs via `{% embed %}` / iframe.

The GLB models here are **web-optimized** derivatives generated from internal
CAD/STEP or Blender source files. No STEP/CAD source, assembly metadata, or
proprietary design data is published here — only the visual mesh geometry
needed for the interactive viewer.

| key | model | source | geometry |
| --- | --- | --- | --- |
| `21224-assembly` | 21224-00 Sensor Assembly | STEP | Draco + mesh-simplified |
| `6x-dovetail` | 6X Smart Dovetail Forward | STEP | Draco + mesh-simplified |
| `65r` | 65R Sensor | Blender GLB | Draco only (full fidelity, multi-material) |

## Structure

```
index.html              landing page
viewer.html             the embeddable viewer (?model=<key>)
embed/<key>.html        dedicated per-model embed page
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

1. Convert the source to GLB and compress it into `models/`:
   - **STEP source:** convert with `cascadio`, then
     `npx @gltf-transform/cli optimize in.glb out.glb --compress draco`.
   - **Blender GLB source:** strip any studio props (e.g. `Backdrop`, `Floor`
     planes) first — otherwise they dominate the scene bounds and break camera
     framing — then `npx @gltf-transform/cli draco in.glb out.glb`. Avoid
     `optimize`/`simplify` on multi-material models so part colors and
     boundaries stay intact.
2. Add a `hotspots/hotspots-<key>.json` file (array of `{id, label,
   description, position, normal}`).
3. Register the model in the `MODELS` map in `viewer.html`, and add a
   dedicated `embed/<key>.html` page.
4. Register the model in the `sentera-3d-viewer` GitBook integration
   (`src/index.tsx` `MODELS` map) and republish so it can be selected in a
   `sentera-3d-model` block.

### Lighting notes

Dark/glossy models (e.g. `65r`) need a higher `exposure` and
`tone-mapping="neutral"`; light monochrome models use a lower `exposure` with
`tone-mapping="commerce"`. Both use `environment-image="neutral"` plus a
gradient page background so the model reads clearly against it.
