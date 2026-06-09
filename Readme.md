# PixelSplit — Sprite Sheet Extractor

A lightweight, browser-based tool that extracts individual sprites from sprite sheets. Drop in an image, get individual sprites out. No installs, no servers, everything runs client-side.

## Features

- **Smart detection** — automatically switches between alpha-based and color-based segmentation depending on whether the image has transparency
- **Exact shape extraction** — cuts out pixel-perfect sprite shapes, not just bounding boxes
- **ZIP support** — accepts ZIP archives as input and packages all extracted sprites into a ZIP for download
- **Configurable** — adjustable alpha threshold, background color picker, minimum sprite size filter, and extraction mode
- **No backend** — everything runs in the browser via Canvas API

## Usage

Just open `sprite-extractor.html` in any modern browser. No build step, no dependencies to install.

```bash
# Or serve it locally if you prefer
npx serve .
```

**Supported formats:** PNG, JPG, WEBP, GIF, BMP, TIFF, SVG, ZIP

## How It Works

1. The image is drawn to a Canvas and pixel data is read
2. A segmentation map is built — pixels are marked foreground or background based on alpha (for transparent images) or color distance from the background color (for opaque images)
3. A flood fill scan finds all connected foreground regions
4. Regions smaller than the minimum size are discarded
5. Each valid region is extracted to its own canvas and displayed

## Settings

| Setting | What it does |
|---|---|
| Alpha Threshold | How opaque a pixel must be to count as foreground (transparent images only) |
| Background Color | The color treated as background — auto-sampled from the top-left pixel, or pick manually (opaque images only) |
| Extraction Mode | **Exact Shape** keeps only the sprite's pixels; **Bounding Box** copies the full rectangular region |
| Minimum Size | Sprites smaller than N×N pixels are discarded as noise |

## Tips

- If sprites are touching each other on the sheet, flood fill will group them as one. Add a pixel or two of padding between them before extracting.
- For opaque backgrounds, results are much better if you remove the background first. The tool auto-detects the top-left pixel as the background color, or you can sample it manually.
- If you're getting lots of tiny fragments, increase the Minimum Size slider.

## Stack

Single HTML file. Vanilla JS, Canvas API, and [JSZip](https://stuk.github.io/jszip/) (loaded from CDN) for ZIP input/output.

## License

MIT