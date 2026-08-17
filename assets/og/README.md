# Open Graph / social share image

`og-psi.png` (1200x630) is the preview card shown when the funnel link is shared
in WhatsApp, on Facebook, or on LinkedIn.

It is **generated**, not hand-drawn. `og-card.html` is the source: it copies the
brand tokens from `index.html` and pulls the same Google Fonts, so the card cannot
drift off-brand. It is excluded from search via a `noindex` meta tag.

## Re-rendering after an edit

Serve the repo, render the card at 2x in a real browser, then downscale so the
type is properly antialiased:

```bash
python3 -m http.server 8765 &
node -e "
const {chromium}=require('playwright-core');
(async()=>{const b=await chromium.launch({args:['--no-sandbox']});
const p=await b.newPage({viewport:{width:1200,height:630},deviceScaleFactor:2});
await p.goto('http://localhost:8765/assets/og/og-card.html',{waitUntil:'load'});
await p.evaluate(()=>document.fonts.ready); await p.waitForTimeout(1200);
await p.screenshot({path:'og-2x.png'}); await b.close();})()"
python3 -c "from PIL import Image; Image.open('og-2x.png').resize((1200,630), Image.LANCZOS).convert('RGB').save('assets/og/og-psi.png', optimize=True)"
```

Keep it at exactly 1200x630 and under 8MB, or WhatsApp will refuse to show it.
