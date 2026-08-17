# Go-live runbook

## Blocker — the funnel will not convert until this is done

### Set the WhatsApp number

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

   The funnel's domain is **`chat.gasmarketing.co.za`**. DNS for `gasmarketing.co.za`
   is at domains.co.za (nameservers `ns1`-`ns4.anycast-ns.com/.net`), not at the host,
   so the record is created there:

   | Field | Value |
   |---|---|
   | Type | `CNAME` |
   | Host | `chat` — **not** the full domain; domains.co.za appends the rest |
   | Value | the per-project target Vercel displays, e.g. `<hash>.vercel-dns-017.com` |

   A subdomain always takes a CNAME. An **A** record is only for an apex domain.
   Do not change the nameservers, and leave the apex, `www` (Webflow) and `MX`
   (Google Workspace) records alone — this addition cannot affect the main site
   or email.

   Propagation is usually minutes, occasionally up to 48 hours. HTTPS is issued
   automatically once DNS resolves — no certificate to buy or install.

5. **Point ad traffic at the new domain** and only then decommission the Netlify site. Keep
   Netlify live until the new domain has served real traffic successfully.

---

## Post-launch checks

- [ ] Every WhatsApp CTA opens a real chat with the pre-filled message intact
- [ ] The social-proof marquee scrolls its wordmarks smoothly
- [ ] The `gas-mark` background image appears in all four places it is used
- [ ] Test on a real phone, not just a narrow desktop window
- [ ] Sharing the link in WhatsApp shows a title, description, and preview image
- [ ] `https://` works and `http://` redirects to it
- [ ] Anchor links in the nav scroll to the right sections without tucking under the fixed bar

## Still outstanding

- **Analytics / conversion tracking.** No Meta Pixel, GA4, or Google Ads tag is installed,
  so paid campaigns pointed here have no conversion signal to optimise against. The
  natural conversion event is a tap on any `[data-wa]` element.
- **Larger favicon master.** The icon set is built from a 120x120 source, so the 192 and
  512 sizes are upscaled and slightly soft. See `assets/brand/README.md`.

## Decommissioning Netlify

Safe once `chat.gasmarketing.co.za` serves from Vercel. DNS and email are at
domains.co.za and the main site runs on Webflow, so Netlify holds nothing but the old
funnel deployment. Before deleting, confirm no live ad or existing link still points at
the old `*.netlify.app` URL, and export anything worth keeping from it.

## Rolling back

Vercel keeps every deployment. Project → Deployments → pick a known-good one →
**Promote to Production**. Recovery is a few seconds and needs no git work.
