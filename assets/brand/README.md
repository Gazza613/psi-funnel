# Brand marks and favicons

| File | Use |
|---|---|
| `gas-mark.png` | Source mark, orange circle on an opaque **white** square. Used as a CSS background in 4 places in `index.html`, where `border-radius:50%` hides the white corners. |
| `gas-mark-transparent.png` | Same mark with a real alpha channel. Source for every icon below. |
| `favicon-16.png`, `favicon-32.png` | Browser tab. `../../favicon.ico` at the repo root carries 16/32/48 for older clients. |
| `apple-touch-icon.png` | iOS home screen. **Deliberately opaque** on brand navy with 12% padding — iOS discards alpha and composites on black, so a transparent icon looks clipped. |
| `icon-192.png`, `icon-512.png` | Android / PWA, referenced from `../../site.webmanifest`. |

## How the transparency was made

The wordmark is **white**, so keying out white by colour would punch holes
straight through the G, A and S. Instead the circle is located by its *orange*
pixels only (which also ignores any grey drop shadow), and an 8x supersampled
circular mask is applied as the alpha channel. Transparent pixels keep an
orange RGB value rather than white, so downscaling never bleeds a pale halo
onto a dark browser tab.

## Regenerating at higher quality

The current icons derive from a 120x120 source, so 192 and 512 are upscaled and
slightly soft. Drop a larger master (512px or above) in here and rebuild the set
from it for crisper large sizes.
