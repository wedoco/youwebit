# youwebit.com

Marketing site for Youwebit. Single static page (`index.html`), Tailwind CDN + Alpine.js CDN, no build step.

## Publish on GitHub Pages

Repo: `git@github.com:wedoco/youwebit.git` (public).

1. Push this folder to `main` (see *First push* below).
2. Repo → Settings → Pages → Source: *Deploy from a branch*, branch `main`, folder `/ (root)`.
3. The `CNAME` file already points to `www.youwebit.com`, so Pages picks the custom domain up
   from the push. Confirm it in Settings → Pages → Custom domain, then tick *Enforce HTTPS*
   once the certificate is issued (can take up to ~1 h after DNS propagates).

Canonical host is `www.youwebit.com`. The apex `youwebit.com` redirects to it — GitHub Pages
does that automatically, but only if the apex A records below exist.

## First push

    git init -b main
    git add .
    git commit -m "Youwebit marketing site"
    git remote add origin git@github.com:wedoco/youwebit.git
    git push -u origin main

## DNS at the registrar

| Type  | Host | Value             | Purpose |
|-------|------|-------------------|---------|
| CNAME | www  | `wedoco.github.io` | serves the site |
| A     | @    | 185.199.108.153   | apex → www redirect |
| A     | @    | 185.199.109.153   | apex → www redirect |
| A     | @    | 185.199.110.153   | apex → www redirect |
| A     | @    | 185.199.111.153   | apex → www redirect |

The `www` CNAME points at `wedoco.github.io` — the org root, **not** `wedoco.github.io/youwebit`
and not the org site's custom domain `www.wedoco.io`.

If Settings → Pages shows `InvalidCNAMEError` right after setup, it is usually just the check
having run before the record propagated. Wait for the TTL and press *Check again* before
changing any record.

Optional IPv6 (AAAA on `@`): `2606:50c0:8000::153` through `...8003::153` —
check https://docs.github.com/pages for the current list before adding.

Verify once propagated:

    dig +short www.youwebit.com
    dig +short youwebit.com

## Email

`hola@youwebit.com` is a Microsoft 365 **shared mailbox** in the same tenant as `info@wedoco.io`.
Shared mailboxes need no licence, so it adds no cost. *Send As* is granted to the wedoco.io user
account, so replies go out as `hola@youwebit.com`.

GoDaddy no longer offers free email forwarding (its Email tab only sells mailboxes), which is why
this uses the existing tenant rather than a relay.

DNS at GoDaddy, **in addition to** the web records above — none of these touch `@` A records or
the `www` CNAME:

| Type  | Name                  | Value                                            | Priority |
|-------|-----------------------|--------------------------------------------------|----------|
| MX    | `@`                   | `youwebit-com.mail.protection.outlook.com`       | 0        |
| TXT   | `@`                   | `v=spf1 include:spf.protection.outlook.com -all` | —        |
| TXT   | `@`                   | `MS=ms…` — verification, from the admin centre   | —        |
| CNAME | `autodiscover`        | `autodiscover.outlook.com`                       | —        |
| CNAME | `selector1._domainkey`| from the DKIM page in Defender                   | —        |
| CNAME | `selector2._domainkey`| from the DKIM page in Defender                   | —        |

`_dmarc` on youwebit.com is already `p=quarantine` (GoDaddy's default). Relaxed SPF alignment
alone satisfies it, but enable DKIM as well — DKIM survives forwarding, SPF does not.

Known gaps on the sibling domain: `wedoco.io` has **no DKIM and no DMARC record**. Unrelated to
this site, but worth fixing separately.

## Local preview

    python3 -m http.server 8080

then open http://localhost:8080

## Editing copy

All text lives in the `COPY` object at the bottom of `index.html` (Spanish `es` and English `en`). The contact address is the `email` field in `site()`.
