# Simply Sacred

Landing page for Danita Briggs, birth doula.

**Live (review URL):** https://hassang97.github.io/danita/

Hand-written static HTML and CSS. No build step, no dependencies, no JavaScript.

## Files

```
index.html      the whole page — every section lives here
styles.css      all styling, mobile breakpoints at the bottom
images/         photos, referenced directly by filename
next_steps.md   outstanding work and known gaps
```

## Local preview

Open `index.html` in a browser. That's it.

To match how Pages serves it (root-relative paths, no `file://` quirks):

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

## Deploying

Pages builds from the **`master`** branch, root directory. Push and it redeploys:

```bash
git push origin master
```

Takes about a minute. Check the build:

```bash
gh api repos/Hassang97/danita/pages/builds/latest --jq '.status'
```

The repo must stay **public** — Pages on a private repo needs GitHub Pro ($4/mo).

## Notes

- **Bump the stylesheet version when you change CSS.** `index.html` links
  `styles.css?v=N`. GitHub Pages serves CSS with `max-age=600`, and browsers that
  already hold a copy will keep using it — increment `N` so the URL changes and the
  new file is fetched.

- **No contact form.** The "Send an Email" button is a `mailto:` link. GitHub Pages is
  static hosting and cannot process a form submission; a real one needs a third-party
  endpoint like Formspree. See `next_steps.md`.
- **Layout.** The services grid is explicitly 3 columns above 900px and 1 below —
  deliberately not `auto-fit`, which orphaned the third card. Cards are flex columns so
  the pricing line pins to the bottom and aligns across all three.
- **Images** are cropped with `object-fit: cover` plus an `object-position` around 25–35%
  to keep faces in frame. Swapping a photo may need that percentage retuned.
- **The `* { margin: 0 }` reset** zeroes paragraph margins, so spacing is restored per
  section with `p + p` rules. New sections with multiple paragraphs will need the same.
- Breakpoints: 900px (services grid), 768px (main mobile layout), 700px (gallery),
  420px (small phones).

## Before launch

See `next_steps.md`.
