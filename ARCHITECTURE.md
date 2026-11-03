# ARCHITECTURE

## What this repository is

The one-page public site of Hedge Quantitative Research LLC at `https://hedgequantresearch.com`, served by GitHub
Pages from the `main` branch of this repository at `/`. Static files only: no framework, no build step, no
JavaScript, no analytics, no cookies, no external scripts, no external fonts. Created 2026-11-03 under the work
order "hedgequantresearch.com v0".

## Files

| File | Role |
| --- | --- |
| `index.html` | The page. Every word of visible text comes from the copy file `SITE_COPY_v0.md` kept outside this repository; this repository does not edit copy. |
| `thanks.html` | The one-line page the contact form redirects to after a successful submission. `noindex`. |
| `style.css` | The only stylesheet, shared by both pages. System serif stack (Georgia), one accent colour for links and the button, hairline rules. |
| `CNAME` | The custom domain for GitHub Pages (`hedgequantresearch.com`). Added when the custom domain was switched on. |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are, without the Jekyll build. |
| `.gitignore` | Ignores `_review/` (screenshots and critiques from the self-review loop) and `.DS_Store`. |
| `README.md` | Two lines. |
| `ARCHITECTURE.md` | This file. |

Relative links only (`style.css`), so the page works both at `https://jlee2000.github.io/hedge-site/` and at the apex
domain.

## How the page deploys

1. Commit to `main` and push. GitHub Pages rebuilds within about a minute (the deployment is the `pages build and
   deployment` workflow GitHub runs on its own; this repository has no workflow files).
2. Pages was enabled with the REST API:
   `gh api -X POST /repos/jlee2000/hedge-site/pages -f 'source[branch]=main' -f 'source[path]=/'`.
3. Check: `curl -sSI https://hedgequantresearch.com` returns `200` and `curl -sSI http://hedgequantresearch.com`
   returns `301` to `https://`.

## Contact form

The form posts to Web3Forms (`https://api.web3forms.com/submit`, method `POST`) with hidden fields `access_key` (the
public access key; it is meant to sit in the page markup), `subject`, `from_name`, `redirect` (absolute URL of
`thanks.html`; Web3Forms requires an absolute `https://` URL on the same domain), and the service's honeypot checkbox
`botcheck`, hidden and left empty. The visible fields are `name`, `email` and `message`; Web3Forms uses the `email`
field as the reply-to address by default. No captcha widget, because one would load external JavaScript. The
destination inbox is configured inside the Web3Forms account, not in this repository.

## DNS

The domain's DNS is hosted at Squarespace Domains (nameservers `nsb1` to `nsb4.squarespacedns.com`; the domain came
over from Google Workspace) and carries the firm's Google Workspace mail.

State before the change, read with `dig` on 2026-11-03 at 11:20 ET:

| Type | Host | Value | What it is |
| --- | --- | --- | --- |
| `A` | `@` | `198.185.159.144`, `198.185.159.145`, `198.49.23.144`, `198.49.23.145` | Squarespace's preset parking records |
| `CNAME` | `www` | `ext-sq.squarespace.com` | Squarespace's preset parking record |
| `MX` | `@` | `1 smtp.google.com` | the firm's mail; not touched |
| `TXT` | `@` | `v=spf1 include:_spf.google.com ~all` | SPF for the mail; not touched |
| `TXT` | `google._domainkey` | `v=DKIM1; ...` | DKIM for the mail; not touched |
| `TXT` | `_dmarc` | `v=DMARC1; p=none;` | DMARC for the mail; not touched |
| `AAAA` | `@` | none | |

The edit J made in Squarespace's DNS settings on 2026-11-03 between 11:50 and 11:53 ET, and nothing else:

1. Remove the two preset web-host records: the four `A` records on `@` listed above and the `www` `CNAME` to
   `ext-sq.squarespace.com`. Squarespace shows them under "Squarespace Defaults". If the panel only offers to delete
   the defaults as a group, delete the group, then confirm the `MX`, SPF, DKIM and DMARC records are still listed; if
   deleting the group would also remove any of those, delete the `A` and `CNAME` records one by one instead. The
   work order asked for an add-only edit; this removal is the one departure, and it is needed because leaving the
   preset `A` records in place would make the apex alternate between GitHub and Squarespace on every lookup.
2. Add the GitHub Pages records:

   | Type | Host | Value |
   | --- | --- | --- |
   | `A` | `@` | `185.199.108.153` |
   | `A` | `@` | `185.199.109.153` |
   | `A` | `@` | `185.199.110.153` |
   | `A` | `@` | `185.199.111.153` |
   | `CNAME` | `www` | `jlee2000.github.io` |
   | `AAAA` (optional) | `@` | `2606:50c0:8000::153` |
   | `AAAA` (optional) | `@` | `2606:50c0:8001::153` |
   | `AAAA` (optional) | `@` | `2606:50c0:8002::153` |
   | `AAAA` (optional) | `@` | `2606:50c0:8003::153` |

3. Do not touch the `MX` record, the SPF `TXT`, the DKIM `TXT` at `google._domainkey`, the DMARC `TXT` at `_dmarc`,
   or any `google-site-verification` `TXT`.

Values verified against GitHub's "Managing a custom domain for your GitHub Pages site" page on 2026-11-03. Check
with `dig +short hedgequantresearch.com A` and `dig +short www.hedgequantresearch.com CNAME`. State after the edit,
read at the authoritative nameservers `nsb1` and `nsb2` at 11:53: the four GitHub `A` records, `www` to
`jlee2000.github.io`, no `AAAA`, and the `MX`, SPF, DKIM and DMARC records unchanged. Record TTL is 14400 seconds, so
a resolver that cached the parking records before the edit can keep returning them for up to four hours.

On the GitHub side, the custom domain is set on the Pages settings
(`gh api -X PUT /repos/jlee2000/hedge-site/pages -f cname=hedgequantresearch.com`), which also writes the `CNAME`
file. GitHub's docs say to set the domain in the repository before changing DNS, so that nobody else can claim the
domain on Pages in between; the setting is harmless while DNS still points elsewhere. GitHub then checks DNS,
provisions a Let's Encrypt certificate, and `https_enforced` is switched on once the certificate state reads
`approved`.

## Review loop

Each round renders screenshots with the installed Google Chrome in headless mode against the local files, then a
written critique against the design brief, then fixes. Screenshots and critiques live under `_review/round-N/`
(ignored by git). The final round's `desktop.png` (1280x800) and `mobile.png` (390x844) are what J reviewed.

Headless Chrome (156 on macOS) clamps `--window-size` widths below 500 to 500, so the phone render goes through a
wrapper page holding a 390 px wide `iframe`, and the capture is cropped to the frame:

```
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
mkdir -p _review/round-N
"$CHROME" --headless --disable-gpu --hide-scrollbars --window-size=1280,800 \
  --screenshot=_review/round-N/desktop.png "file://$PWD/index.html"
cat > _review/frame-mobile.html <<'HTML'
<!DOCTYPE html><html><head><meta charset="utf-8"><style>html,body{margin:0;padding:0;background:#fff}iframe{border:0;display:block}</style></head><body><iframe src="../index.html" width="390" height="844"></iframe></body></html>
HTML
"$CHROME" --headless --disable-gpu --hide-scrollbars --window-size=600,1000 \
  --screenshot=_review/round-N/mobile.tmp.png "file://$PWD/_review/frame-mobile.html"
python3 -c "from PIL import Image; Image.open('_review/round-N/mobile.tmp.png').crop((0,0,390,844)).save('_review/round-N/mobile.png')"
```

## Deliberately absent

No copy beyond the copy file, no services menu, no team page, no testimonials, no client names, no blog, no pricing,
no visible email address (the form is the contact mechanism; no `mailto:`), no analytics, no cookie notice, no
newsletter capture, no social icons beyond the one LinkedIn text link, no images, no icons, no favicon (the
`data:` icon link only stops the browser requesting one), no JavaScript, no framework, no CI.

## Change log

- 2026-11-03 11:31 ET: repository created; v0 page, thanks page, stylesheet; Pages enabled on `main` at `/`
  (`a06e30c`, `53bca42`).
- 2026-11-03 11:41: copy revision from the copy file: "Massachusetts" without the town in the subtitle, footer and
  description; thank-you line (`b584064`).
- 2026-11-03 11:53: `CNAME` file; form redirect, canonical and `og:url` moved to the custom domain (`8050238`);
  custom domain set on Pages; certificate approved for the apex and `www` at 11:53 (expires 2027-02-01);
  `https_enforced` switched on at 11:54.
