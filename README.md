# Abiotic Factor — Facility Map

Interactive top-down viewer of Abiotic Factor's connected facility areas,
sliced horizontally every 5 meters. Pan, zoom, flip between floors, drop pins
on rooms worth remembering.

Live link (once hosted): `https://pauloaguiar.github.io/abiotic-map/`

## Features

- 40 floor layers from `z = +50 m` down to `z = -150 m` in 5 m slabs
- **Progressive hi-res**: low-res PNGs load instantly; each layer upgrades to
  its hi-res copy in the background the first time you visit it
- **Onion skin** shows ±2 above and +4 below faded under the current layer
- **Symbols**: add named pins at (x, y, layer), searchable sidebar, survives
  reload via `localStorage`, shareable via `symbols.json`
- Keyboard + wheel navigation

## Controls

| Action | Binding |
|---|---|
| Pan | drag |
| Zoom | mouse wheel |
| Recenter | double-click |
| Next / prev layer | **Ctrl + wheel**, Shift + wheel, **↑ / ↓**, **PageUp / PageDown** |
| Jump to layer | click its thumbnail in the bottom strip |
| Add a symbol | click **+ Add**, then click on the map |
| Open symbol | click its row in the sidebar (map centers on it) |
| Edit / delete symbol | double-click the row, or right-click the pin |
| Save symbols to file | **Save** in sidebar → downloads `symbols.json` |
| Load symbols from file | **Load** in sidebar → pick a `symbols.json` |
| Reset to shipped defaults | **Reset** in sidebar (asks first) |

## Layout

```
.
├── final_viewer.html    # the app (single file, no build)
├── manifest.json        # lists the layers (and any default symbols)
├── layers/              # low-res PNGs (fast initial load)
└── hires_layers/        # hi-res PNGs (progressive upgrade, optional)
```

## manifest.json

```json
{
  "title": "Abiotic Factor — Facility Map",
  "layers": [
    { "name": "z +50..+45",
      "file":  "layers/r2048_00_z _50.._45.png",
      "hires": "hires_layers/hires_00_z _50.._45.png" }
  ],
  "symbols": [
    { "name": "Elevator",      "layer": 10, "x": 1024, "y": 512, "color": "#ff5252" },
    { "name": "Med bay",       "layer":  7, "x":  700, "y": 400, "color": "#40c4ff" }
  ]
}
```

- `layers[].hires` is optional — if absent the low-res stays in place.
- `symbols[]` is an optional default set. First-time visitors seed their
  `localStorage` from it. After that, each visitor's edits live in their own
  browser; the **Reset** button in the sidebar wipes their local copy and
  re-seeds from whatever is currently in `manifest.json`.
- The viewer parses the first signed number in each layer's `name` for the
  bottom-up sort order and picks the `z ≈ 0` layer as the default view.