# AnglerBook Website

Marketing + legal site at **anglerbook.fun** (GitHub Pages, CNAME; DNS at Infomaniak).
Local clone: ~/Developer/AnglerBook_website (underscore locally; remote is hyphenated
GLYSK-OU/AnglerBook-website via redirect).

## Pages
- index.html     Landing.
- support.html   Public support page (App Store submission requirement).
- marketing.html Internal marketing page.
- privacy.html   Privacy policy — linked from the app paywall (App Review 3.1.2).
- terms.html     Terms of use — linked from the app paywall.
- 404.html
- /catalog/      Redirects to catalog.anglerbook.fun (canonical gear data lives there; never
                 duplicated here).

## Notes
- Footer: Support link + beta CTA (iOS TestFlight link, Android placeholder).
- "GLYSK OÜ" kept unbroken (non-breaking space) across pages.
- Light/dark/auto theme toggle. Deploy notes: DEPLOY.md, SETUP-statichost-infomaniak.md.

## Deploy = GitHub Pages
Commit to main → Pages builds. Verify via `gh api repos/GLYSK-OU/AnglerBook-website/pages/builds/latest`
(status + SHA).

## Commit discipline
"Just do it" — commit + push directly.
