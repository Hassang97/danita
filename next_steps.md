# Next Steps

Checklist for finalizing the Simply Sacred landing page.

## Open

- [ ] **GitHub Pages still shows "DNS check unsuccessful"** (`NotServedByPagesError`) on the
  repo's Pages settings. It is cosmetic: all four a2hosting nameservers return the GitHub IPs,
  the site serves 200, and Enforce HTTPS is on (GitHub only allows that once the cert exists).
  The checker is hitting a resolver cache still holding the old `66.198.240.27` record under
  its original 14400s TTL — those expire by roughly 10:00 UTC on 2026-08-23. Click
  **Check again** after that and it should go green.

- [ ] **Transfer the repo to Danita's GitHub account.** The site currently lives under
  `Hassang97`, which means one person's account controls what is served at the business's
  domain. The code itself is safe — the repo is public and the site is static, so it can always
  be recovered — but only the owner can push. If that account is unreachable, suspended, or
  deleted, the site freezes as-is (no fixing a price, no pulling a testimonial) or disappears
  entirely, and her domain points at nothing.

  **Plan:** Danita creates a GitHub account; transfer `danita` to it
  (Settings -> General -> Danger Zone -> Transfer ownership); add `Hassang97` back as a
  collaborator so day-to-day pushes are unchanged. Note that adding her as a collaborator on
  the existing repo is *not* equivalent — on a personal repo, collaborators can push but
  cannot transfer or delete. A free GitHub organization owned by her, with both as owners,
  is the alternative if two people need full rights.

  **After transferring:** the Pages site rebuilds under the new owner and the `CNAME` file
  carries over, so DNS needs no change — but confirm https://simplysacred.life still loads and
  that Enforce HTTPS is still on. GitHub redirects the old repo URL, but update the URL
  recorded in any handover notes anyway.

  **Also record for her, wherever the handover notes live:** the repo URL, and the one line a
  stranger would need — "static HTML site on GitHub Pages, DNS managed at hosting.com."

## Known gaps

- [x] **No contact form — decided, not a gap.** There was one, but it never worked: it posted
  to `action="#"`, which silently discarded every submission and reloaded the page. It was
  replaced with a `mailto:` button, which works with no backend. Opening the visitor's email
  client is the accepted behaviour; a real form would need a third-party endpoint (Formspree,
  or Netlify Forms if the site ever moved there) and is not wanted. Revisit only if `mailto:`
  proves to lose enquiries.
- [ ] **Images are unoptimized.** 1.55 MB total, every photo served at full resolution
  (~1100x1600) regardless of the size it displays at. The gallery is `loading="lazy"`, but the
  About, services, and contact photos all load up front (~740 KB). Resizing to roughly 2x their
  displayed size and/or serving WebP would cut this substantially.
- [ ] **No service area anywhere on the page.** The copy never says what region Danita covers,
  and the phone number's 289 area code is the only hint. Anyone searching locally has no way to
  tell if she serves them. Worth adding to the contact section and the meta description.

## Launch

_Complete — the site is live at https://simplysacred.life._

## Future: consolidate on Cloudflare

Goal: cut recurring cost from roughly $450/yr to roughly $30/yr, and leave Danita with as few
logins as possible. Not urgent — see the timing note below.

**Target setup**

| Piece | Today | Target | Cost |
|---|---|---|---|
| Web hosting | A2 "Ignite" cPanel, ~$400/yr | GitHub Pages (already serving) | $0 |
| DNS zone | a2hosting nameservers | Cloudflare DNS | $0 |
| Domain | hosting.com, ~$50/yr | Cloudflare Registrar (at-cost) | ~$30/yr |
| Email @simplysacred.life | cPanel mailboxes | Cloudflare Email Routing -> Outlook, if needed | $0 |

Verify the `.life` renewal price before committing; the figure above is an estimate.

**Keep hosting on GitHub Pages.** Cloudflare Pages was considered and rejected: the source has
to live in git either way, so moving the host does not remove the GitHub account from the
picture and therefore saves no logins. Two accounts is the floor — Cloudflare for the name and
the plumbing, GitHub for the site.

**The one thing that must not be done out of order:** the DNS zone lives *on the A2 hosting
account*. Cancelling the plan destroys the zone, the domain stops resolving, and the site goes
dark even though it is hosted on GitHub. Move DNS first, always.

**Order of operations**

1. cPanel -> Email Accounts: find out whether any real `@simplysacred.life` mailboxes exist and
   are used. The business card uses `simply.sacred@outlook.com`, so there may be none. If there
   are, export the mail — cancelling deletes it.
2. Add the domain to Cloudflare, let it import the existing zone, then change the nameservers
   at the registrar. Confirm https://simplysacred.life still loads before going further.
3. Only if step 1 found live mailboxes: set up Cloudflare Email Routing to forward to Outlook.
   It forwards but cannot *send* from the address; Zoho Mail's free tier can, if that matters.
4. Cancel the A2 hosting plan.
5. Transfer the domain to Cloudflare Registrar. Independent of the above. Needs the transfer
   lock off (currently `Locked: Yes`) and the EPP code — both on the domain's General page at
   hosting.com.

**Timing.** The hosting product shows **renews 2028-02-12** and the domain **2027-02-13**, so
the hosting looks prepaid for roughly three more years. If so, cancelling early saves nothing
and probably refunds nothing — the saving is in not renewing in 2028. Confirm with billing what
was actually paid for and whether any of it is refundable. Even so, do steps 1-3 well before
then: moving DNS off a box that is about to be abandoned is the risky part, and it is far
easier done deliberately than under a deadline.

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
