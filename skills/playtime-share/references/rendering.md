# Rendering HTML to PDF / PNG / JPEG

We use **headless Chrome** as the primary renderer. It's pre-installed on most macOS machines (Google Chrome) and reproduces print exactly.

## Detection — find the browser

Try these in order:

1. macOS Chrome: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
2. macOS Chromium: `/Applications/Chromium.app/Contents/MacOS/Chromium`
3. macOS Brave: `/Applications/Brave Browser.app/Contents/MacOS/Brave Browser`
4. Linux Chromium: `chromium` or `chromium-browser` on PATH
5. Linux Chrome: `google-chrome` on PATH

If none are found, fall back to `npx -y puppeteer` (slow first run, downloads chromium).

Save the resolved path so subsequent renders are fast.

## HTML → PDF

```bash
"$CHROME" \
  --headless=new \
  --disable-gpu \
  --no-sandbox \
  --print-to-pdf="$OUTPUT_PDF" \
  --print-to-pdf-no-header \
  --no-pdf-header-footer \
  --virtual-time-budget=10000 \
  "file://$INPUT_HTML"
```

Notes:
- `--no-pdf-header-footer` removes the URL/date footer Chrome otherwise adds.
- `--virtual-time-budget=10000` waits 10s for fonts/images.
- For paper size, the source HTML's `@page` rule controls it. Don't pass `--print-to-pdf-paper-size`; let CSS win.

## HTML → PNG (full page screenshot)

```bash
"$CHROME" \
  --headless=new \
  --disable-gpu \
  --no-sandbox \
  --hide-scrollbars \
  --window-size=1700,2200 \
  --screenshot="$OUTPUT_PNG" \
  --virtual-time-budget=10000 \
  "file://$INPUT_HTML"
```

For a US Letter page at 2x DPI: `--window-size=1700,2200`. For A4 at 2x: `--window-size=1654,2339`.

## HTML → JPEG

Headless Chrome only outputs PNG. For JPEG, render to PNG then convert with `sips` (macOS) or `convert` (ImageMagick):

```bash
sips -s format jpeg -s formatOptions 88 "$OUTPUT_PNG" --out "$OUTPUT_JPG"
```

Or, if ImageMagick is installed:

```bash
convert "$OUTPUT_PNG" -quality 88 "$OUTPUT_JPG"
```

## HTML → Share card (1080×1080 PNG)

The share-card HTML is composed from `share-card-template.html` with placeholders filled. Then:

```bash
"$CHROME" \
  --headless=new \
  --disable-gpu \
  --no-sandbox \
  --hide-scrollbars \
  --window-size=1080,1080 \
  --default-background-color=00000000 \
  --screenshot="$OUTPUT_CARD_PNG" \
  --virtual-time-budget=5000 \
  "file://$SHARE_CARD_HTML"
```

## Multi-page packets

If the packet has page-break CSS and renders to >1 page, the PDF handles it natively. For PNG, generate a separate PNG per page by:

1. Rendering the full PDF first
2. Using `sips` (macOS) or `pdftoppm` (Linux) to extract each page as PNG:

```bash
# macOS
sips -s format png "$OUTPUT_PDF" --out "${OUTPUT_PNG_PREFIX}-1.png"
# Linux
pdftoppm -png -r 200 "$OUTPUT_PDF" "${OUTPUT_PNG_PREFIX}"
```

## Stripping the affiliate footer

For PNG / JPEG / share card outputs, the source HTML has a `<div class="affiliate-footer">` block. Before rendering:

1. Read the source HTML
2. Make a temp copy
3. Remove the affiliate footer block (regex or simple string replace on the marker comment `<!-- AFFILIATE-FOOTER:START -->` ... `<!-- AFFILIATE-FOOTER:END -->`)
4. Render the temp copy
5. Delete temp file

The print template in `playtime-print` should always wrap affiliate content with those markers so this strip works reliably.

## Troubleshooting

- **Fonts wrong**: Increase `--virtual-time-budget` or embed fonts directly in the HTML (preferred for offline forever).
- **Output file is empty / 0 bytes**: Chrome silently fails when path has spaces — quote it. Also check the user has write access to the output directory.
- **macOS prompts for permission**: First run of headless Chrome may trigger a "Chrome wants to access folders" dialog. Once approved, subsequent runs are silent.
- **Linux missing libs**: Install `libnss3`, `libatk-bridge2.0-0`, `libgbm1`, `libxss1`, `libasound2`.
