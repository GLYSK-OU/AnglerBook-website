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

---

## AnglerBook vault — documentation contract

The Obsidian vault is the **master** for cross-repo facts: architecture decisions, contracts,
issue reasoning, process. This file is the master for **repo-local** facts: file layout, build
commands, local gotchas. Where the two disagree, **the vault wins** — fix this file, do not fork
the fact.

Vault: `01 - Projects/01.01 - AnglerBook/` — source of truth `vault.lopes.me`, read/write through
the `Obsidian_Vault-WEB_MCP` connector, never raw SSH or shell writes.

### The six fields

| Field | Holds |
|---|---|
| `01.01.01 - Core — Cross Platform/` | engineering both apps share: backends, catalog, domain contract, localisation policy |
| `01.01.02 - iOS/` | iOS development — ten mirrored topic slots |
| `01.01.03 - Android/` | Android development — the same ten slots |
| `01.01.04 - Bugs & Wishes/` | one note per defect or request, keyed by GitHub issue number |
| `01.01.05 - Marketing & Campaigns/` | not development — campaigns, brand, channels, site |
| `01.01.06 - Operations & Governance/` | repos, roadmap, audits, vault upkeep |

`01.01.00 - AnglerBook.md` is the hub and the only file at the folder root. `_assets/` and
`_superseded/` are infrastructure — never numbered, never a write target.

The ten slots, identical on both platforms: 01 Architecture · 02 Data & Persistence ·
03 Identity & Auth · 04 Sync · 05 Monetisation · 06 Localisation · 07 Companion & Widgets ·
08 Platform Services · 09 Build, Versioning & Release · 10 Testing & QA. **The number is a fixed
topic slot, not an alphabetical position** — slot N is the same subject on iOS and Android, so a
missing counterpart is visible at a glance.

### The four binding contracts

A change to any of these must land in **both** platform folders in the same session:

- `01.01.01.09 - Domain Model — Canonical Entities` — the entity contract. Platform persistence
  notes are implementations of it, never a second source.
- `01.01.01.01 - Backend — Fishing Buddies`
- `01.01.01.02 - Backend — Telemetry Architecture` — the wire contract binds both clients. Never
  change it unilaterally from one repo, regardless of that repo's commit discipline.
- `01.01.01.08 - Catalog — Source of Truth & Client Wiring`

### Where truth lives

- **Issue status → GitHub.** Issues in the component code repo; pipeline on the org board
  **AnglerBook Tracker - Dashboard** (org project 2,
  `https://github.com/orgs/GLYSK-OU/projects/2`). Never call it "Project #2". There is no
  `AnglerBook-Tracker` repository in the model.
- **Reasoning → the vault.** Root cause, the decision, the lesson. Never let the vault become a
  second status tracker, and never let this file become a second decision log.
- **Repo-local mechanics → this file.**

### Every session that touches AnglerBook

ends with a vault write to the right field — what was decided, what changed, what is next.
Read the destination folder's `.00` index before writing; confirm every write with the full path
and byte count.

### Commit gate — non-negotiable

- `AnglerBook-iOS` (and its `AnglerBook-WatchOS` submodule) and `AnglerBook-Android`: stage the
  diff and present it for **explicit approval** before ANY commit; **no push** without a separate
  explicit approval. Documentation and this file included.
- `AnglerBook-catalog`, `anglerbook-telemetry`, `AnglerBook-website`, `AnglerBook-buddies`,
  `AnglerBook-Marketplace`, `AnglerBook-Marketing_Campaign`: just do it.

### The repos

`AnglerBook-iOS` · `AnglerBook-Android` · `AnglerBook-WatchOS` (git submodule of iOS) ·
`AnglerBook-catalog` · `AnglerBook-website` · `anglerbook-telemetry` · `AnglerBook-buddies` ·
`AnglerBook-Marketplace` · `AnglerBook-Marketing_Campaign` (published marketing dashboard).
