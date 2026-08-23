# Next Steps

Checklist for finalizing the Simply Sacred landing page.

## Blocking — needs your input

_Nothing outstanding._

## Known gaps

- [ ] **No contact form.** There was one, but it never worked: it originally posted to
  `action="#"`, which silently discarded every submission and reloaded the page. It has
  been removed in favour of a `mailto:` button, which works with no backend.
  **GitHub Pages is static hosting and cannot process a form submission itself** — a real
  form requires a third-party endpoint such as Formspree (free tier ~50 submissions/month),
  which means swapping the form's `action` for the id they issue.
- [ ] **Page metadata is thin.** `<title>` is still the generic "Doula Services | Supporting
  Your Journey" — no brand name. There is no `<meta name="description">`, no favicon, and no
  Open Graph tags, so links shared to Instagram/Facebook will preview with no image or blurb.
- [ ] **Images are unoptimized.** 1.72 MB total, every photo served at full resolution
  (~1100x1600) regardless of the size it displays at. The gallery is `loading="lazy"`, but the
  About, services, and contact photos all load up front (~740 KB). Resizing to roughly 2x their
  displayed size and/or serving WebP would cut this substantially.
- [ ] **`images/business-card.jpeg` is unused,** deliberately — it is a phone snapshot of the
  card held in hand over a chair, which reads as out of place next to the professional
  photography. Kept in the repo in case a clean scan replaces it.

## Launch

- [ ] **Go live:** complete the DNS switch for `simplysacred.life` to point at GitHub Pages.

## Done

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
