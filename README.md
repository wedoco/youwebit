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

| Type  | Host | Value              | Purpose |
|-------|------|--------------------|---------|
| CNAME | www  | `wedoco.github.io` | serves the site |
| A     | @    | 185.199.108.153    | apex → www redirect |
| A     | @    | 185.199.109.153    | apex → www redirect |
| A     | @    | 185.199.110.153    | apex → www redirect |
| A     | @    | 185.199.111.153    | apex → www redirect |

Delete any parking/forwarding record the registrar put on `@` or `www` at purchase, or it will
shadow these. Optional IPv6 (AAAA on `@`): `2606:50c0:8000::153` through `...8003::153` —
check https://docs.github.com/pages for the current list before adding.

Verify once propagated:

    dig +short www.youwebit.com
    dig +short youwebit.com

## Local preview

    python3 -m http.server 8080

then open http://localhost:8080

## Editing copy

All text lives in the `COPY` object at the bottom of `index.html` (Spanish `es` and English `en`). The contact address is the `email` field in `site()`.
