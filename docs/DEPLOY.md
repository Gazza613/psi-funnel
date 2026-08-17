# Go-live runbook

## Blockers — the funnel will underperform until these are done

### 1. Set the WhatsApp number

`index.html`, in the script block near the bottom:

```js
var WA_NUMBER = '';   // ← empty
```

Every WhatsApp CTA on the page (the nav button, two in-page launchers, and the floating
bubble) reads this value. While it is empty the site deliberately does **not** open a broken
chat — it shows a "our WhatsApp line is switching on" toast instead. That is graceful, but it
converts nothing.

Format: **country code + number, digits only, no `+`, no spaces, no leading zero.**

```js
var WA_NUMBER = '27821234567';   // South Africa, 082 123 4567
```

### 2. Add the nine client logos

`assets/logos/` is empty. `index.html` expects these exact filenames:

```
mtn-momo.png        wh-properties.png    chilla.png
learnalot.png       fedex.png            old-mutual.png
hyundai.png         the-amber-room.png   sun-international.png
```

Missing files do not break the page — each `<img>` has an `onerror` handler that swaps in
the client's name as text. But the logo marquee is the social-proof block, so it is worth
having the real marks. Transparent PNG, roughly 200px wide, lowercase filenames.

---

## First-time deploy

1. **Push this repo to GitHub.**

   ```bash
   git add -A
   git commit -m "Restructure for Vercel hosting"
   git push origin main
   ```

2. **Import into Vercel.** At [vercel.com/new](https://vercel.com/new), connect the GitHub
   account and pick `psi-funnel`. Settings:

   - Framework Preset: **Other**
   - Root Directory: `./`
   - Build Command: *leave empty*
   - Output Directory: *leave empty*

   There is no build step — Vercel serves the repo root as static files.

3. **Deploy.** You get a `psi-funnel-*.vercel.app` URL in under a minute. Check it before
   attaching a domain.

4. **Attach the domain.** Project → Settings → Domains → Add. Vercel shows the DNS records
   to create at your registrar:

   - Apex (`psi.example.com` style root): an **A** record to Vercel's IP
   - Subdomain (`www` or `psi`): a **CNAME** to `cname.vercel-dns.com`

   Propagation is usually minutes, occasionally up to 48 hours. HTTPS is issued
   automatically once DNS resolves — no certificate to buy or install.

5. **Point ad traffic at the new domain** and only then decommission the Netlify site. Keep
   Netlify live until the new domain has served real traffic successfully.

---

## Post-launch checks

- [ ] Every WhatsApp CTA opens a real chat with the pre-filled message intact
- [ ] All nine client logos render — no text fallbacks visible
- [ ] The `gas-mark` background image appears in all four places it is used
- [ ] Test on a real phone, not just a narrow desktop window
- [ ] Sharing the link in WhatsApp shows a title, description, and preview image
- [ ] `https://` works and `http://` redirects to it
- [ ] Anchor links in the nav scroll to the right sections without tucking under the fixed bar

## Still outstanding

- **Favicon.** The page has none, so browsers show a blank tab icon.
- **Open Graph tags.** No `og:title`, `og:description`, or `og:image`, which means links
  shared in WhatsApp and on Facebook render as a bare URL with no preview card. For a
  paid-media funnel this is a measurable click-through loss.
- **Canonical URL.** Needs the live domain before it can be set.
- **`sitemap.xml` and `robots.txt`** contain a `REPLACE-WITH-YOUR-DOMAIN` placeholder.
- **Analytics / conversion tracking.** No Meta Pixel, GA4, or Google Ads tag is installed,
  so paid campaigns pointed here currently have no conversion signal to optimise against.

## Rolling back

Vercel keeps every deployment. Project → Deployments → pick a known-good one →
**Promote to Production**. Recovery is a few seconds and needs no git work.
