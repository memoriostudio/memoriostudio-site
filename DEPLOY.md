# memoriostudio.com — deployment

**LIVE:** https://memoriostudio.com — since 2026-08-19.
**Repository:** https://github.com/memoriostudio/memoriostudio-site
**Host:** GitHub Pages, serving `main` from the repository root. No Vercel, no build step,
no dependencies. Every push to `main` republishes within a minute.

---

## How the domain reaches this repository

The domain is registered at **GoDaddy**, but its nameservers are delegated to **Cloudflare** —
so records are edited in the Cloudflare dashboard, never at the registrar. Anyone who goes
looking in GoDaddy will find a zone that does nothing.

| Type | Name | Value | Proxy |
|---|---|---|---|
| A | @ | 185.199.108.153 | **DNS only** |
| A | @ | 185.199.109.153 | **DNS only** |
| A | @ | 185.199.110.153 | **DNS only** |
| A | @ | 185.199.111.153 | **DNS only** |
| CNAME | www | memoriostudio.github.io | **DNS only** |

All four apex IPs are needed; they are GitHub Pages' published addresses.

**Grey cloud, not orange, and that is deliberate.** GitHub Pages issues and renews its own
Let's Encrypt certificate by reaching the apex directly. Proxying these records through
Cloudflare breaks that renewal, and it breaks it silently — months later, when the certificate
comes up for renewal rather than the day the toggle is flipped.

**Do not touch the MX or TXT records in that zone.** Google Workspace mail for the domain lives
there, alongside SPF, DMARC and the Resend/SES DKIM records. The cutover left all of them alone.

`CNAME` in the repository root holds `memoriostudio.com`; that file is what tells GitHub Pages the
apex belongs to this repository. It is committed, and deleting it un-binds the domain.
**HTTPS is enforced**, so `http://` and `www.` both 301 to `https://memoriostudio.com/`.

### What it replaced

The apex previously pointed at a Vercel deployment (`216.150.1.1`, with `www` →
`ae4cb208cea46e65.vercel-dns-017.com`) serving a "Memorial portraits — launching soon" page.
Only the DNS moved. **The Vercel project still exists and was not modified.**

### Verifying from the command line

```
dig +short memoriostudio.com A
dig +short www.memoriostudio.com
dig +short memoriostudio.com MX          # must still be Google's five
curl -sI https://memoriostudio.com/ | head -1
```

---

## Still outstanding for Apple

1. ~~**Create `contact@memoriostudio.com`.**~~ **Done** — it exists in Google Workspace (confirmed by
   the founder, 2026-08-19). It is the only address the site publishes, in every footer, in the
   `Organization` JSON-LD on all six pages, and in `security.txt`.

   **The enrollment address is a different address and does not need to appear here.** Apple's
   requirement is that the enrollment email's *domain* match the organization's website domain — not
   that the address itself be published. Enrollment used `<firstname>@memoriostudio.com`, which
   satisfies it. **Apple writes to the enrollment address**, so that inbox is the one to watch during
   review; `contact@` is for the public.
2. **Consider adding the business phone** from the D&B record — verification often involves a call.
3. **Retire or repoint the old Vercel project.** DNS no longer sends anyone to it, but the project
   still claims `memoriostudio.com` on the Vercel side — so a future DNS mistake, or someone
   re-adding the domain there, puts the old page back. Note it is a real application with Stripe
   webhooks and Twilio A2P paths, so it was deliberately left untouched rather than deleted; the
   safe move is to remove the domain from that project, not the project itself.

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
