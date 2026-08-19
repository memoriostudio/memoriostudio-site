# memoriostudio.com — deploy notes

**What this is:** the parent-company site for **Memorio Studio LLC**, built to satisfy Apple
Developer Program *organization* enrollment, which requires a website that is "publicly available
and functional" on a domain associated with the organization, and explicitly rejects "websites that
contain minimal content" or registrar/placeholder pages.

**Deliberately says nothing about the product.** It presents the studio, not OORBO. No unreleased
product names, no screenshots, no claims the product has not earned.

## Files

Plain static HTML + one stylesheet. **No build step, no dependencies, no JavaScript.**
Deployable to Vercel, Netlify, Cloudflare Pages, S3, or any static host.

```
site/index.html      site/about.html    site/contact.html
site/privacy.html    site/terms.html    site/404.html
site/styles.css      site/favicon.svg
site/robots.txt      site/sitemap.xml   site/.well-known/security.txt
```

**Host configuration:** serve `404.html` as the not-found page, and redirect `www` → apex (or the
reverse) so only one hostname is canonical — `<link rel=canonical>` already points at the apex.

Preview locally: `cd site && python3 -m http.server 8123` → http://127.0.0.1:8123

## Before it goes live — founder actions

1. **Take the domain off the old site.** `memoriostudio.com` currently serves a "Memorial
   portraits — launching soon" page behind a password, from the unrelated earlier project. That
   page fails Apple's requirement twice: placeholder content, and a business that does not match
   the enrolling entity. Point the domain at this site instead.
2. **Create one mailbox — `contact@memoriostudio.com`.** It is the only address on the site.
   Apple also requires the enrollment email itself to be **on the organization's domain**, so a
   working mailbox is a hard prerequisite, not a nicety.
3. **Fill three facts this repo does not hold.** All are left general rather than invented:
   - **State of organization** — for the governing-law line in `terms.html`.
   - **Business mailing address** — worth adding. Apple cross-references the enrolling entity
     against its D&B record; an address on the site that matches that record is the strongest
     available signal that the domain belongs to the organization. Drop it into the company ledger
     on `/` and `/about.html`, and into the `PostalAddress` in the JSON-LD block.
   - **Business phone** — same reasoning; Apple's verification often involves a call.
4. **Deploy, then verify from outside** — load all four pages in a private window with no VPN, on
   the bare domain and `www`, and confirm no password gate remains.

## What deliberately is NOT here

- **No product or security claims.** Earlier drafts described on-device storage and an
  advertising-free model. Those were cut: they describe an unreleased product, they are detail the
  founder does not want public yet, and a claim made before it can be demonstrated is the exact
  failure this workspace treats as unacceptable everywhere else. The privacy policy still states
  what the *website* does, because that is verifiable by loading it.
- No analytics, no cookies, no trackers, no embedded third-party content.
- No product marketing, no waitlist, no email capture.
- No EIN, no D-U-N-S, no bank details. Company identity is public; company secrets are not.


## Quality bar, verified

Checked headlessly on every page (`/`, `/about`, `/contact`, `/privacy`, `/terms`, `/404`):

- **WCAG AA contrast** — zero text below 4.5:1. The first pass had seven failures, including the
  call-to-action at 3.73:1; the steel and muted tokens were darkened until every page measured clean.
- **No horizontal overflow** at 1440px, 1280px and 390px.
- **Zero JavaScript** — so zero JavaScript errors, and nothing to break in a reviewer's browser.
- **Every internal link resolves 200.**
- `lang`, one `<h1>` per page, ordered headings, skip link, visible focus rings.
- `canonical`, Open Graph and Twitter meta, `Organization` JSON-LD, `sitemap.xml`,
  `robots.txt`, RFC 9116 `security.txt`, print stylesheet for the legal pages.

**Word counts** (the "minimal content" rejection is the one to fear): home 309, about 327,
privacy 450, terms 324, contact 118.
