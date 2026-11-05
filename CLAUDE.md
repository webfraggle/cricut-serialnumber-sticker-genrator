# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file offline web app `seriennummern-generator.html` (German UI) that generates serial-number sticker sheets for Cricut **Print Then Cut**: an SVG with only the inner cut grid and a 1200-dpi PNG with the labels, both exactly the same size (default 4 × 47 labels of 40 × 5 mm = 160 × 235 mm). Requirements and design decisions: `docs/konzept.md`.

No build, no dependencies, no network: open the HTML file directly in Chrome/Firefox (`open seriennummern-generator.html`). Keep it that way — no CDNs or external libraries.

## Architecture (all inside the one `<script>`)

- **DataMatrix** (between `// ==DATAMATRIX-START==` / `// ==DATAMATRIX-END==`): self-written ECC200 encoder (ASCII mode, square 10×10…26×26, GF(256) poly 301, ISO 16022 Annex F placement). The markers allow extracting it for tests in Node.
- `layout()` derives cols/rows from usable area ÷ label size; numbering is **column-wise** (index → `col = floor(i/rows)`, `row = i % rows`).
- `drawSheet(ctx, settings, layout, pxPerMm)` is the single renderer used for both preview and PNG export; font size is uniform per sheet, fitted to the longest label. Data Matrix encodes only the number (no prefix), right-aligned.
- `buildSvg()` emits one `<path>` with inner lines only (no outer border — Cricut cuts the PNG outline; the PNG therefore has an opaque white background).
- `setPngDpi()` inserts a pHYs chunk so the PNG carries its dpi.
- localStorage: settings under `seriennummernGenerator.v1`; next start number under `….nextStart`, set after PNG export, cleared when the start field is edited manually.

## Testing

No test suite. Verify Data Matrix changes by extracting the encoder between the markers in Node (`new Function(src + '; return DataMatrix;')`) and decoding rendered symbols with `zxing-cpp` (Python). Known vector: `"123456"` → codewords `142 164 186 114 25 5 88 102`.
