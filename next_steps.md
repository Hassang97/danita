# Next Steps

Checklist for finalizing the Simply Sacred landing page.

## Open

- [ ] **GitHub Pages still shows "DNS check unsuccessful"** (`NotServedByPagesError`) on the
  repo's Pages settings. It is cosmetic: all four a2hosting nameservers return the GitHub IPs,
  the site serves 200, and Enforce HTTPS is on (GitHub only allows that once the cert exists).
  The checker is hitting a resolver cache still holding the old `66.198.240.27` record under
  its original 14400s TTL — those expire by roughly 10:00 UTC on 2026-08-23. Click
  **Check again** after that and it should go green.

## Known gaps

- [ ] **No contact form.** There was one, but it never worked: it originally posted to
  `action="#"`, which silently discarded every submission and reloaded the page. It has
  been removed in favour of a `mailto:` button, which works with no backend.
  **GitHub Pages is static hosting and cannot process a form submission itself** — a real
  form requires a third-party endpoint such as Formspree (free tier ~50 submissions/month),
  which means swapping the form's `action` for the id they issue.
- [ ] **Images are unoptimized.** 1.55 MB total, every photo served at full resolution
  (~1100x1600) regardless of the size it displays at. The gallery is `loading="lazy"`, but the
  About, services, and contact photos all load up front (~740 KB). Resizing to roughly 2x their
  displayed size and/or serving WebP would cut this substantially.
- [ ] **No service area anywhere on the page.** The copy never says what region Danita covers,
  and the phone number's 289 area code is the only hint. Anyone searching locally has no way to
  tell if she serves them. Worth adding to the contact section and the meta description.

## Launch

_Complete — the site is live at https://simplysacred.life._

## Done

- [x] **Page metadata and share previews.** Title is now "Simply Sacred | Birth & Postpartum
      Doula", with a meta description, canonical URL, theme colour, favicon
      (`favicon.ico` + `images/apple-touch-icon.png`, sage "S" monogram), and full Open Graph
      and Twitter card tags. `images/og-image.jpg` (1200x630) is generated from the About
      portrait on the site's warm-sand background.
      **Preview caching, as observed:** WhatsApp builds previews on the *sending device*, so
      iPhone (never having scraped the domain) shows the correct preview immediately, while
      WhatsApp Desktop still shows the old cPanel page ("Danta Briggs" + cP logo) from its own
      local cache — that clears on logout or ages out by itself. Facebook and Instagram use a
      shared server-side cache instead, so run the URL through the Sharing Debugger at
      developers.facebook.com/tools/debug and hit "Scrape Again" *before* posting there,
      or everyone will be served the cPanel version.
- [x] **Live at `https://simplysacred.life`.** Apex A records in the a2hosting cPanel Zone
      Editor point at GitHub Pages (`185.199.108-111.153`, TTL 300); `www` stays a CNAME to
      the apex. A `CNAME` file in the repo root sets the custom domain, and GitHub
      auto-enabled Enforce HTTPS once Let's Encrypt issued the cert.
- [x] All other zone records (`mail`, `ftp`, `cpanel`, `webmail`, MX, SPF, DKIM, DMARC) still
      point at the A2 host `66.198.240.27` and were left untouched. **The A2 hosting plan is
      still active and still serves an older site** — check what depends on it before cancelling.

- [x] Real photos throughout — About, all three service cards, contact, and a six-image gallery.
- [x] Services grid renders three across instead of silently collapsing to two with an orphan card.
- [x] Service cards are equal height with headings and pricing lines aligned.
- [x] Contact section rebuilt as one card with the photo and details aligned.
- [x] Mobile breakpoints at 768px and 420px (there were none).
- [x] Smooth anchor scrolling and eased button/link/focus transitions, with a
      `prefers-reduced-motion` guard.
- [x] Three real client testimonials in place; the two placeholder ones removed.
- [x] Email address set to `simply.sacred@outlook.com`, matching the business card.
- [x] Paragraph spacing restored — the `* { margin: 0 }` reset had collapsed the About
      section's four paragraphs into one solid block.
