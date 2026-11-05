# ARCHITECTURE

## What this repository is

The one-page public site of Hedge Quantitative Research LLC at `https://hedgequantresearch.com`, served by GitHub
Pages from the `main` branch of this repository at `/`. Static files only: no framework, no build step, no
JavaScript, no analytics, no cookies, no external scripts, no external fonts. Created 2026-11-03 under the work
order "hedgequantresearch.com v0".

## Files

| File | Role |
| --- | --- |
| `index.html` | The page. Every word of visible text comes from the copy file `SITE_COPY_v0.md` kept outside this repository, except the "Writing" line in the Public section, which comes from the work order of 2026-11-04; this repository does not edit copy. |
| `thanks.html` | The one-line page the contact form redirects to after a successful submission. `noindex`. |
| `style.css` | The only stylesheet, shared by every page. System serif stack (Georgia), one accent colour for links and the button, hairline rules. The data page's rules (headings, file list, posting-dates table) sit in their own block and use selectors the home page does not carry, so the home page renders unchanged. |
| `data/index.html` | The data and methods page at `/data/`. Its text comes from the work order of 2026-11-04 and the public reports it names, not from the copy file. |
| `data/ma_mlr_four_payers_2014_2023.csv` | The table behind the explainer's chart (40 rows). See "data/" below. |
| `data/README_ma_mlr_four_payers.txt` | Plain-text read-me for the table: method, columns, CMS sources with sha256, exclusions, attribution rule, license. |
| `data/puf_release_dates.csv` | When CMS posted each of its ten MLR public use files (10 rows). |
| `CNAME` | The custom domain for GitHub Pages (`hedgequantresearch.com`). Added when the custom domain was switched on. |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are, without the Jekyll build. |
| `.gitignore` | Ignores `_review/` (screenshots and critiques from the self-review loop) and `.DS_Store`. |
| `README.md` | Two lines. |
| `ARCHITECTURE.md` | This file. |

Relative links only (`style.css`; `../style.css` and `../` from the data page), so the pages work both at
`https://jlee2000.github.io/hedge-site/` and at the apex domain. One exception: the home page's Writing line links
`/data/`, root-relative, as the work order wrote it; it resolves on the apex domain, and the github.io URL redirects
there.

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

## data/

The data and methods bundle the explainer "How to use the CMS Medicare Advantage MLR public use files" cites (Substack,
for 2026-11-12). Added 2026-11-04 under the work order of that day. Three data files and one page:

- `ma_mlr_four_payers_2014_2023.csv`: the Medicare Advantage MLR of UnitedHealth Group, Humana, CVS Health (Aetna) and
  Elevance Health (Anthem), 2014 to 2023, one row per payer and year. A reformatted copy of the parent-year table of the
  public-data build in the firm's private research repository, which reads CMS's public files only. The copy drops
  the two deprecated thin-year sensitivity columns (suppressed county cells imputed at 5), spells the payers out, and
  renames six columns for a public reader (the numerator and denominator, CMS's published-basis sums and ratio, and
  claims over gross revenue); every other column keeps its name. Ratios to six decimals, dollars to the cent, counts
  as integers, UTF-8, LF, header row; rows ordered by payer, then year.
- `README_ma_mlr_four_payers.txt`: wraps at 100 except the eight CMS URLs too long for a line. Its URLs and sha256
  values were copied by script from the build's lineage record, not retyped. The method paragraphs are the chart's
  own wording and the build's contract-weights methods paragraph.
- `puf_release_dates.csv`: the build's posting-date record of the ten MLR files, without its internal match column
  and with two notes reworded to drop references to the build's own folders.
- `index.html`: the methods note (files, the table and the chart, posting dates, sensitivities, corrections line). Its
  posting-dates table is generated from `puf_release_dates.csv`; on screens under 40rem each range breaks before
  "to", and the table sits in a horizontally scrolling wrapper as a backstop.

Every number on the page and in the read-me comes from the build's published reports of 2026-11-03 and 2026-11-04,
not from memory.

**Scrub list.** Before each commit, every file under `data/` and every line added elsewhere is searched for the
client's name in either case, the capitalised client and licensing markers, the names of the two private
repositories, the private repository's folder, work-order and hand-off labels, the former town, the statutory dataset
and its vendor (the bundle names no dataset other than CMS's files), the old SEC user-agent string, and the em-dash and
en-dash characters. Every count must be zero. Internal commit SHAs, file paths and tool names do not appear. The
client, if it must be referred to at all, is "a national value-based care company"; the bundle does not refer to it.

**No code.** Nothing executable is published: `data/` holds the three data files and the page. The conversions above
ran as one-off scripts outside both repositories.

**License.** "Data: CC BY 4.0, Hedge Quantitative Research LLC. Underlying CMS files are United States government
works." on the page and in the read-me (the work order's default; J may overrule before pushing).

## Review loop

Each round renders screenshots with the installed Google Chrome in headless mode against the local files, then a
written critique against the design brief, then fixes. Screenshots and critiques live under `_review/round-N/`
(ignored by git). The final round's `desktop.png` (1280x800) and `mobile.png` (390x844) are what J reviewed. Round 4 (2026-11-04)
renders the data page as `data-desktop.png`, `data-desktop-full.png`, `data-mobile.png` and `data-mobile-full.png`
(frames `_review/frame-data-mobile.html` and `_review/frame-data-mobile-full.html`, iframe heights 844 and 6000; full
captures trimmed to the content), and the home page as `desktop.png` and `mobile.png`.

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

No copy beyond the copy file (the Writing line and the data page excepted, as above), no services menu, no team page,
no testimonials, no client names, no blog (the Writing line points to posts published elsewhere), no pricing, no
visible email address on the home page (the form is the contact mechanism; no `mailto:` anywhere; the data page's
corrections line carries the firm's address as plain text, as the work order of 2026-11-04 wrote it), no analytics, no cookie notice, no
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
- 2026-11-04 19:40 ET: `data/` (the explainer's table, the posting-date record, the read-me and the methods page) and
  the stylesheet's data-page block; README line (commit "data: the explainer's table, posting-date record and methods
  note under /data/", `df93b3e`). Not pushed: J reviews the rendered pages first.
- 2026-11-04 19:45: home page Writing line in the Public section, after the CFTC link, with the `/data/` link only
  (no Substack URL yet; J adds the post's link in a one-line commit when it exists) (commit "home: Writing line with
  the /data/ link"). Not pushed.
