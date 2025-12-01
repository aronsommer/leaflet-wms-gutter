# Leaflet WMS Gutter

Add gutter support to Leaflet's WMS tile layers to prevent icons and symbols from being cut off at tile boundaries.

> Developed for [OpenMapEditor](https://github.com/openmapeditor/openmapeditor) and released as a standalone plugin for the Leaflet community.

## The Problem

WMS layers with point symbols or icons often have visual artifacts where symbols are clipped at tile edges. This happens because each tile is rendered independently by the WMS server.

## The Solution

This plugin implements OpenLayers-style gutter rendering:

- Requests slightly larger tiles (with overlap)
- Uses canvas to crop the gutter area
- Symbols spanning tile boundaries are fully rendered

## Usage

```javascript
// Instead of L.tileLayer.wms()
const wmsLayer = L.tileLayer.wms.gutter("https://example.com/wms", {
  layers: "my-layer",
  format: "image/png",
  transparent: true,
  tileSize: 512,
  gutter: 64, // 64px overlap on each side
});

map.addLayer(wmsLayer);
```

## Installation

Download `leaflet-wms-gutter.js` and include it after Leaflet:

```html
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="leaflet-wms-gutter.js"></script>
```

## How It Works

1. Expands the BBOX by `resolution × gutter` map units
2. Requests tiles with dimensions `tileSize + (2 × gutter)`
3. Uses canvas `drawImage()` to crop and display only the center portion
4. Icons in the overlap area prevent cutoff at boundaries

## Options

| Option   | Type   | Default | Description                                       |
| -------- | ------ | ------- | ------------------------------------------------- |
| `gutter` | Number | 0       | Pixels of overlap on each side (typically 32-128) |

All standard `L.tileLayer.wms` options are supported.

## Example

See the included `index.html` for a working example, or view the [live demo](https://aronsommer.github.io/leaflet-wms-gutter/).

## Browser Support

Works in all modern browsers that support Canvas 2D.

## Performance

The canvas rendering has minimal performance impact. For layers without gutter (gutter=0), the plugin falls back to standard `<img>` tile rendering.

## Credits

Inspired by OpenLayers' TileWMS gutter implementation. This plugin brings the same functionality to Leaflet.

## References

- [OpenLayers TileWMS Gutter Implementation](https://github.com/openlayers/openlayers/blob/main/src/ol/source/TileWMS.js) - The original inspiration for this plugin
- [OpenLayers Canvas Tile Renderer](https://github.com/openlayers/openlayers/blob/main/src/ol/renderer/canvas/TileLayer.js) - Canvas cropping technique

## License

MIT License - Copyright (C) 2025 Aron Sommer

See LICENSE section in the source code for full details.
