# AnglerBook.fun — deploy notes

Static site. No build step — these files serve as-is.

## Files
- index.html ............ landing page (brand crest, features, Pro, footer)
- terms.html ............ Terms of Use (GLYSK OÜ, Estonia governing law)
- privacy.html .......... Privacy Policy (App Store privacy-policy URL)
- catalog/index.html .... redirect → GitHub-hosted gear.json
- 404.html .............. branded not-found page
- og-image.png .......... social share card (1200×630)
- favicon.svg
- assets/ ............... style.css + self-hosted fonts (no third-party CDN)
- SETUP-statichost-infomaniak.md ... full GitHub → StaticHost → Infomaniak guide

## Catalog redirect target
/catalog currently 302s to:
  https://raw.githubusercontent.com/GLYSK-OU/AnglerBook/main/catalog/gear.json
To change it, edit the URL in catalog/index.html (it appears in the meta-refresh,
the visible link, and the <script> fallback — one target, three spots).

## Deploy to statichost (direct upload)
The statichost site must already exist in the dashboard, and anglerbook.fun must be
attached to it as a custom domain (Add site / Domains — there is no API for that yet).

    export STATICHOST_APIKEY='your_token'
    shcli <your-site-name> .       # run from inside this folder; uploads contents

## DNS for anglerbook.fun
Point the domain at the IP statichost shows for the site in the dashboard
(A record on the apex, or the CNAME they give you). First hit after attaching the
domain takes ~1–2s while the TLS cert provisions — expected.

## Before launch
- Contact address is development@glysk.eu (set in terms.html and privacy.html).
- Privacy Policy is live at /privacy.html — use this as the App Store privacy URL.
- Confirm the privacy.html assumptions still hold: no third-party advertising/analytics
  SDKs, GLYSK runs no content servers, weather source is Apple WeatherKit.
