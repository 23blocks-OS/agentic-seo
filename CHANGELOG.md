# Changelog

## 2.1.1

- Description now says when to use the skill, so it triggers for SEO work and stays quiet otherwise.
- Version moved under `metadata`; added `allowed-tools` and `compatibility`.

## 2.1.0 - 2026-10-05

### Added
- `audit-build` sub-command: audit every page of a built site, with an optional diff against the build that is live now (`scripts/audit_build.py`, `reference/audit-build.md`). Python 3 standard library only.
  - Per-page checks from `audit.md`, run on built HTML.
  - Sitemap and redirect reconciliation: an indexable page missing from the sitemap, a `noindex` page in it, a sitemap URL with no page, a redirect source that is still built (the static page wins, so the rule never fires), a redirect target that is not built, duplicate titles.
  - `--baseline`: URLs removed and added; changes to index state, canonical, title, description and JSON-LD. Each failure is marked new or inherited.
  - `--strict-claims`: fail JSON-LD that carries price, rating, review or availability terms.
  - `--self-test`: a clean fixture must raise nothing and every planted defect must fire.
- Guidance on reporting evidence: a numbers line, the method, VERIFIED / REASONED / OPEN, and a positive control for any negative result.

### Changed
- `audit.md`: `keywords`, `twitter:site` / `twitter:creator` (when the site has no handle) and the schema count are advisory. A missing `alt` fails; `alt=""` is valid for decoration and is listed for a human to confirm.
- `structured-data.md`: the `offers` template ships `price: '0'` and "Free tier available". It is now marked optional.

## 2.0.0
- Removed all hardcoded site references; the skill is fully generic.

## 1.0.0
- Initial release: `audit`, `implement`, `structured-data`, `llms-txt`, `checklist`.
