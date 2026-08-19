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
site/index.html  site/privacy.html  site/terms.html  site/contact.html
site/styles.css  site/favicon.svg   site/robots.txt
```

Preview locally: `cd site && python3 -m http.server 8123` → http://127.0.0.1:8123

## Before it goes live — founder actions

1. **Take the domain off the old site.** `memoriostudio.com` currently serves a "Memorial
   portraits — launching soon" page behind a password, from the unrelated earlier project. That
   page fails Apple's requirement twice: placeholder content, and a business that does not match
   the enrolling entity. Point the domain at this site instead.
2. **Create one mailbox — `contact@memoriostudio.com`.** It is the only address on the site.
   Apple also requires the enrollment email itself to be **on the organization's domain**, so a
   working mailbox is a hard prerequisite, not a nicety.
3. **Fill two facts this repo does not hold:** the state of organization (for the governing-law
   line in `terms.html`) and, if you want it public, a business mailing address. Both were left
   general rather than invented.
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
