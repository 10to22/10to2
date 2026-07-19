# Merch product photos

The store (CD Wizard + mobile) shows a front/back image viewer for each
apparel item. These files are currently **placeholders** — replace each with
the real Printful mockup (or any square-ish product photo) using the **exact
same filename**, and the store picks them up automatically. No code changes.

| File | Product | Which shot |
|------|---------|------------|
| `tee-front.png`    | 10to2 logo tee (white) | front |
| `tee-back.png`     | 10to2 logo tee (white) | back |
| `hoodie-front.png` | 10to2 zip hoodie       | front |
| `hoodie-back.png`  | 10to2 zip hoodie       | back |
| `jersey-front.png` | 10to2 jersey           | front |
| `jersey-back.png`  | 10to2 jersey           | back |
| `flag-front.png`   | 10to2 wall flag        | (single) |

## How to replace them (easiest — GitHub, no tools)

1. Open this folder on GitHub: `assets/merch/`
2. Click **Add file → Upload files**.
3. Drag in your 7 photos, renamed to match the filenames above exactly
   (`.png` or change the paths in index.html if you use `.jpg`).
4. Commit. GitHub Pages redeploys in ~1 min and the store shows them.

Notes
- One front + one back per product covers **every size** — the mockup is
  identical across S–5XL, so there's no need for per-size images.
- Keep them reasonably sized (Printful mockups are ~1000×1000 — perfect).
  Square images display best in the viewer.
- To get the exact Printful mockups: Printful dashboard → the product →
  right-click each mockup → Save image, or use the "Download mockups" option.
