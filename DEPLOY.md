# memoriostudio.com — deployment

**LIVE NOW:** https://memoriostudio.github.io/memoriostudio-site/
**Repository:** https://github.com/memoriostudio/memoriostudio-site
**Host:** GitHub Pages, serving `main` from the repository root. No Vercel, no build step,
no dependencies. Every push to `main` republishes within a minute.

---

## To put it on memoriostudio.com — the only step left

The domain is registered at **GoDaddy** and currently points at the old Vercel project
(the "Memorial portraits — launching soon" page). Two changes move it here.

### 1 · GoDaddy DNS

In GoDaddy → *My Products* → the domain → **DNS** → *Manage Zones*.

**Delete** the existing `A` record(s) on `@` and the `CNAME` on `www` that point at the old host,
then add:

| Type | Name | Value | TTL |
|---|---|---|---|
| A | @ | 185.199.108.153 | 1 hour |
| A | @ | 185.199.109.153 | 1 hour |
| A | @ | 185.199.110.153 | 1 hour |
| A | @ | 185.199.111.153 | 1 hour |
| CNAME | www | memoriostudio.github.io | 1 hour |

Those four IPs are GitHub Pages' published apex addresses. All four are needed.

### 2 · Tell GitHub the domain is ours

Repository → **Settings → Pages → Custom domain** → enter `memoriostudio.com` → Save.
Then tick **Enforce HTTPS** once the certificate is issued (usually minutes, up to an hour).

That writes a `CNAME` file back into the repository, which is why one is not committed here:
while a CNAME claims a domain that still points elsewhere, GitHub redirects the preview URL to
that domain — and the site becomes impossible to verify. It was removed for exactly that reason.

### 3 · Check from outside

Private window, no VPN, both `memoriostudio.com` and `www.memoriostudio.com`. Confirm the
password gate is gone and all five pages load.

---

## Still outstanding for Apple

1. **Create `contact@memoriostudio.com`.** It is the only address on the site, and Apple requires
   the enrollment email itself to be on the organization's domain.
2. **Consider adding the business phone** from the D&B record — verification often involves a call.
3. **Retire or repoint the old Vercel project** once the domain has moved, so it cannot serve the
   old site again. Note it is a real application with Stripe webhooks and Twilio A2P paths; it was
   deliberately left untouched.

## What is on the site

Six pages — home, about, contact, privacy, terms, 404 — roughly 2,900 words.
Legal entity, entity type, registered address and jurisdiction appear in the company ledgers, in
every footer, in the JSON-LD `Organization` block, and in the contact sections of both legal
documents. Privacy and terms are written to cover mailing lists, marketing email, cookies and
analytics before any of them exist, so neither needs rewriting the day one is added.

## Quality bar, verified headlessly on every page

- **WCAG AA contrast** — zero text below 4.5:1, measured with translucent layers composited.
- **No horizontal overflow** at 1440px, 1280px and 390px.
- **Zero JavaScript** — nothing to break in a reviewer's browser.
- Every internal link resolves 200, on the live host.
- `lang`, one `<h1>` per page, ordered headings, skip link, visible focus rings.
- `canonical`, Open Graph, Twitter meta, `Organization` JSON-LD, `sitemap.xml`, `robots.txt`,
  RFC 9116 `security.txt`, custom 404, print stylesheet.
