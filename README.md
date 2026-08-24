# openfreemap-expressive

The OpenFreeMap Bright style, rebuilt as parameters and layer modules in
[Expressive](https://github.com/falseinput/expressive), a DSL that compiles to MapLibre GL
style JSON.

119 layers, driven by 15 parameters.

See it running at [falseinput.github.io/expressive](https://falseinput.github.io/expressive),
where every parameter is a control and the style recompiles in the browser.

This project is unaffiliated with OpenFreeMap. The name refers to the upstream style the design
derives from.

## Build

```bash
npm install
npm run build
```

The style lands in `dist/style.light.json`.

## Structure

```
styles/bright/
  style.light.exp     imports the modules
  params/light/
    core.json         the 15 parameters below
    detail.json       colors with no derivable relationship to a core parameter,
                      contrast targets, per-class label sizes, module visibility
  derive/
    tokens.exp        applies the palette modifiers to every base color
    colors.exp        every color in the style, and the halo rule
    sizes.exp         label sizes and road widths
    visibility.exp    the module switches
  layers/             one file per module, each exporting named groups
```

Parameters flow one way: `params` to `derive` to `layers` to the entry file. Change a value in
`params/light/` and rebuild.

## Parameters

Three modifiers reach every color in the palette before anything derives from it, so one control
moves the whole map.

| Parameter | What it does |
| --- | --- |
| `paletteHue` | Rotates every color by this many degrees. Wraps at 360. |
| `paletteSaturation` | Multiplies the saturation of every color. 0 gives greyscale. |
| `paletteLightness` | Adds points to the lightness of every color. -70 reaches a dark map. |

Ten colors anchor a family each. The rest of the palette derives from them.

| Parameter | What it does |
| --- | --- |
| `canvas` | Land surface. Land tints, building outlines, minor casings, and paths derive from it. |
| `water` | Rivers, streams, and canals. Intermittent water derives from it. |
| `vegetation` | Parks and grass. Woodland derives from it. |
| `road` | Trunk, primary, secondary, and tertiary fills. Tunnel fills derive from them. |
| `motorway` | Motorway fills. Every major road casing derives from it. |
| `building` | Building footprints. |
| `boundary` | Country boundaries. Region boundaries derive from it. |
| `label` | Place labels below city rank. Halos take maximum contrast against it. |
| `accent` | Water body labels. |
| `sand` | Sand and beaches. |

Two multipliers scale a whole family while the relative sizes inside it hold.

| Parameter | What it does |
| --- | --- |
| `roadScale` | Multiplies every road width. |
| `labelScale` | Multiplies every label text size. |

Every parameter carries a title, a description, and bounds where it is numeric, so a tool can build
a control for it without a separate configuration.

## Tiles, sprites, and fonts

The style points at tile, sprite, and glyph endpoints run by
[OpenFreeMap](https://openfreemap.org/), which offers them for public use.

Sprite colors are baked, so POI icons, highway shields, and oneway arrows keep Bright's colors
whatever the parameters say. A palette far from the default needs its own sprite.

## License

Terms apply per directory, indexed in [NOTICE.md](NOTICE.md).

`styles/bright/` derives from OSM Bright and carries BSD 3-Clause and CC BY 4.0 terms, preserved in
[styles/bright/LICENSE.md](styles/bright/LICENSE.md). Everything else is MIT.

A map built from this style credits OpenMapTiles and OpenStreetMap:

```
© OpenMapTiles © OpenStreetMap contributors
```
