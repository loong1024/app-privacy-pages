# BulkStamp — public pages (DRAFT)

Support, privacy, and EULA pages for **BulkStamp — Batch Watermark** (`com.appdev1017.bulkstamp`).

**Status: DRAFT — not published, not committed, no GitHub Pages change.** Every page carries a
visible DRAFT banner and `<meta name="robots" content="noindex">`. Do not use any of these URLs in
App Store Connect until the steps below are done and `curl -I` returns `200`.

## Files

| File | Purpose |
|---|---|
| `index.html` | Support page — app intro, 8 FAQs, contact block |
| `privacy.html` | Privacy Policy, converted from `~/projects/bulkmark/docs/privacy-policy.md` (DRAFT preserved) |
| `eula.html` | Terms of Use / EULA, converted from `~/projects/bulkmark/docs/eula.md` (DRAFT preserved) |

Pages are self-contained: inline CSS only, no images, no fonts, no scripts, no external assets.
Dark mode follows `prefers-color-scheme`. Layout is single-column, max-width 720 px; wide tables
scroll horizontally on narrow phones. Footer on every page: `© 2026 BulkStamp` + contact placeholder.

## Deployment path and expected URLs

Repo: `loong1024/app-privacy-pages` (GitHub Pages serves the repository root).
Existing apps live in top-level folders (`shrinkpix/`, `facepop/`, `bdaycountdown/`, …); this page
follows the same shape.

| Page | Expected URL after publish |
|---|---|
| Support (index) | `https://loong1024.github.io/app-privacy-pages/bulkstamp/` |
| Privacy | `https://loong1024.github.io/app-privacy-pages/bulkstamp/privacy.html` |
| EULA | `https://loong1024.github.io/app-privacy-pages/bulkstamp/eula.html` |

Recommended ASC wiring once live: `privacyPolicyUrl` → privacy.html, `supportUrl` → index.html
(the assembly draft proposed using the same URL for both; a dedicated support page is a superset and
also satisfies the "Support URL" field).

### Blocking decision — folder name mismatch

- This work order and this directory use **`bulkstamp/`**.
- `~/projects/bulkmark/docs/asc-assembly-draft.md` (§ item 10, §六) and
  `~/projects/bulkmark/docs/privacy-policy.md` (header) both plan **`bulkmark/`** and the URL
  `https://loong1024.github.io/app-privacy-pages/bulkmark/`.
- Pick one **before publishing**, because `privacyPolicyUrl` is entered in ASC and must resolve at
  review time:
  - **(a)** rename `bulkstamp/` → `bulkmark/` (keeps the assembly draft and privacy-policy header
    valid, zero other edits); or
  - **(b)** keep `bulkstamp/` and update the two planning docs + ASC URLs.

## Before publishing (CEO / engineering to fill)

All placeholders are rendered with a highlighted style and can be found with `grep -o '\[[^]]*\]'`.

| Placeholder | Where | Who / note |
|---|---|---|
| `[CONTACT_EMAIL]` | all three pages, 10 occurrences | CEO. Precedent in `shrinkpix/` is `leonxing01@icloud.com`; confirm whether BulkStamp uses the same address. |
| `[ENTITY_NAME]` | privacy, eula | CEO — legal entity or individual name shown as data controller / licensor. |
| `[EFFECTIVE_DATE]` | privacy, eula | CEO — set to the publish date, not the draft date. |
| `[POSTAL_ADDRESS]` | privacy, eula | CEO — required in some jurisdictions; may be left out only after a legal call. |
| `[RETENTION_PERIOD — CEO/lawyer to confirm, e.g. 24 months]` | privacy §9 | CEO/lawyer. |
| `[GOVERNING_LAW_JURISDICTION]`, `[VENUE]` | eula §15 | CEO/lawyer. |
| `[PRIVACY_POLICY_URL]` | eula §7 | Set to the final privacy URL from the table above (depends on the folder-name decision). |
| `[REPORT_AD_PATH — to be confirmed by engineering]` | privacy §4 | eng-developer — confirm the in-app path (PRD currently: Settings → Report an Ad). |
| `[CONSENT_SETTINGS_PATH — to be confirmed by engineering]` | privacy §6 | eng-developer — confirm the UMP privacy-options entry (PRD: Settings → Ad privacy options). |

`[your name]` inside §7 of the privacy page is Apple's own Settings path, not a placeholder — leave it.

### Open legal decision — custom EULA vs Apple Standard EULA

`asc-assembly-draft.md` §六 currently says "reuse Apple's Standard EULA, do not write a custom one";
`eula.md` is the custom alternative. `eula.html` renders with a visible draft note explaining the
choice. If option (a) is chosen, delete the EULA page (or keep it unlinked) and link Apple's
standard EULA instead; if option (b), the custom EULA must be pasted into App Store Connect's
custom-EULA field (web-only, no API) and linked in-app.

## Publish checklist (only after CEO order)

1. Fill every placeholder in the table above; resolve the folder-name decision.
2. Delete each page's DRAFT banner block and the `<meta name="robots" content="noindex">` line
   (each is marked with a `DRAFT` comment). Also remove the draft note block in `eula.html`.
3. `git add bulkstamp && git commit` (repo is currently on `main`; follow the usual branch/push rule).
4. Push and wait for Pages to rebuild.
5. Verify: `curl -I` each URL returns `200`; body contains "AdMob", "one-time", and the
   photos-stay-on-device statement (assembly checklist item 23).
6. Enter the verified URLs in ASC (`privacyPolicyUrl`, `supportUrl`, and EULA if custom) and record
   the change in `org-ledger/app-launch-registry.md` + `org-ledger/events.md`.

## Validation performed (2026-11-13)

- Tag balance checked on all three pages with Python's `html.parser` — no unclosed or mismatched tags.
- No external CSS/JS/image dependencies; the only absolute URLs are anchor links users may tap
  (Google policies, Apple legal, reportaproblem).
- Repo state: `bulkstamp/` is the only new entry. `fileconverter/index.html` was already modified in
  the working tree before this task and was **not** touched.

## Note on the source documents

`privacy-policy.md` is still marked DRAFT / not reviewed by counsel; its content was converted
faithfully, including all bracketed placeholders. `eula.md` likewise.