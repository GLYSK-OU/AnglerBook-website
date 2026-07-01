# Putting AnglerBook.fun live — GitHub → StaticHost → Infomaniak

The chain is: your **GitHub repo** holds the site, **StaticHost** builds/serves it, and
**anglerbook.fun** (registered at Infomaniak) points at StaticHost via DNS. Once wired,
every `git push` redeploys automatically.

> What can't be automated: StaticHost has no API for creating sites, attaching domains, or
> reading tokens — those are dashboard actions at <https://builder.statichost.eu>. DNS is
> done in the Infomaniak Manager. Everything below is click-through; it's a one-time setup.

---

## 1 — Put the site in a GitHub repo

If the site isn't in its own repo yet, make one (keeping it separate from the app repo is
cleanest). Put the site files **at the repo root** — `index.html` must sit at the top level,
not inside a subfolder.

```sh
cd anglerbook-site            # the folder from the zip
git init
git add .
git commit -m "AnglerBook.fun website"
git branch -M main
git remote add origin git@github.com:GLYSK-OU/anglerbook-fun.git   # create this repo on GitHub first
git push -u origin main
```

Public repo = simplest. If you make it **private**, you'll grant StaticHost read access to it
via its GitHub app during step 2 — that works too.

Final layout in the repo root:

```
index.html  terms.html  privacy.html  404.html  favicon.svg  og-image.png
assets/…    catalog/index.html
```

---

## 2 — Create the StaticHost site, connected to the repo

1. Sign in at <https://builder.statichost.eu> → **Add site**.
2. Choose **Connect a git repository** (not "upload zip").
3. Authorise GitHub if prompted, then pick `GLYSK-OU/anglerbook-fun`, branch **main**.
4. Build settings — this is **plain HTML, no build step**:
   - Build command: leave empty.
   - Publish / public directory: the repo **root** (`/` or `.`).
5. Create the site. StaticHost clones the repo and serves it at
   `https://<your-site>.statichost.eu`. Open that URL and confirm the page renders before
   touching DNS.

**If the dashboard insists on a `statichost.yml`**, add this at the repo root and push it:

```yaml
# statichost.yml — no build, serve the repo as-is
public: .
```

(Only add the `image:` / `command:` keys if you later introduce a real build step. For these
static files, `public: .` is all that's needed.)

---

## 3 — Attach anglerbook.fun in StaticHost

In the site's dashboard → **Domains** (or "Custom domain") → add `anglerbook.fun`
(and `www.anglerbook.fun` if you want both).

StaticHost then shows you the DNS target to use — for the apex domain that's an
**IP address (A record)**. Note it down; you'll paste it into Infomaniak next. Leave this
tab open.

---

## 4 — Point Infomaniak DNS at StaticHost

1. Go to <https://manager.infomaniak.com> → **Domains** → `anglerbook.fun` → **DNS zone**
   (Manage DNS / Gérer la zone DNS).
2. Add / edit these records using the values StaticHost gave you:

   | Type  | Name (host) | Value                          | TTL  |
   |-------|-------------|--------------------------------|------|
   | A     | `@`         | StaticHost IP (from step 3)    | 3600 |
   | CNAME | `www`       | `<your-site>.statichost.eu.`   | 3600 |

   - The apex (`@`) **must** be an A record — DNS doesn't allow a CNAME on the bare domain.
   - If StaticHost gives you a second IP for `www`, use an A record for `www` instead of the
     CNAME. Follow whatever StaticHost displays.
3. **Delete any conflicting records** Infomaniak set by default — a parking/redirect A record
   on `@`, or a "web hosting" alias — otherwise the domain keeps pointing at Infomaniak.
4. Save the zone.

---

## 5 — Turn on auto-deploy (optional but worth it)

So every push to `main` rebuilds the site:

1. GitHub repo → **Settings → Webhooks → Add webhook**.
2. Payload URL: `https://builder.statichost.eu/<your-site>`
3. Content type `application/json`, event: **just the push event**. Save.

The URL itself is the credential — no secret/token needed. To trigger a redeploy by hand
instead, run the bundled script: `rebuild.sh <your-site>`.

---

## 6 — Verify

DNS can take anywhere from minutes to a couple of hours to propagate. Then:

```sh
curl -I https://anglerbook.fun           # expect HTTP/2 200
```

- First request after attaching the domain takes ~1–2s while the TLS certificate provisions —
  that's normal, not an error.
- Check the three routes: `/` (landing), `/terms.html`, `/privacy.html`, and `/catalog`
  (browsable catalog; fetches `https://catalog.anglerbook.fun/catalog/gear.json` at runtime).

---

## Quick reference

- **Add site / attach domain / tokens / build logs** → dashboard, `builder.statichost.eu`.
- **DNS records** → Infomaniak Manager, DNS zone for `anglerbook.fun`.
- **Redeploy** → push to `main` (if webhook set) or `rebuild.sh <site>`.
- **App Store URLs** → Privacy: `https://anglerbook.fun/privacy.html` ·
  Terms: `https://anglerbook.fun/terms.html`.

## If something's off

- **Domain still shows an Infomaniak page** → a default `@` A record or web-hosting alias is
  still in the zone; remove it.
- **`anglerbook.fun` works but `www` doesn't (or vice-versa)** → add the missing record; both
  the apex and `www` need their own entry.
- **Cert warning that doesn't clear after a few minutes** → re-check the A record value
  matches StaticHost exactly, then wait for propagation.
- **Pushes don't redeploy** → webhook URL wrong, or pointing at the wrong `<site>` slug.
