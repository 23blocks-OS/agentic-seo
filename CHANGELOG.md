# Changelog

## 2.2.0 - 2026-10-10

### Changed
- Description no longer claims to follow "Google's AI optimization guide". Replaced the invented "Google AI Optimization Principles" with one dated paragraph quoting the guide (last updated 2026-07-10): no special files or markup are needed for AI features.
- Removed the "at least 2 schema types" rule from `SKILL.md`, `structured-data.md`, `implement.md`, `audit.md` and `checklist.md`. Add structured data only when it matches visible content.
- FAQPage is optional: Google stopped showing FAQ rich results on 2026-05-07. HowTo templates and recommendations removed. Note added that WebPage markup produces no rich result.
- `llms-txt.md` rewritten: Google Search does not use llms.txt; add one only if the owner wants it; no ranking or citation claims. Removed the invented 65-line, 600-line and alphabetical-order rules; format now follows llmstxt.org.
- Sitemap: removed `priority` and `changefreq` (Google ignores them); `lastmod` only when accurate.
- Meta keywords is no longer a required tag anywhere.
- Validation step: run the Rich Results Test and the Schema Markup Validator before reporting done. "Fast load times" now names LCP, INP and CLS.
- `SKILL.md` trimmed from 184 to under 100 lines: the Angular section is now a pointer to `implement.md`; anti-patterns that repeated reference content removed; `audit_build.py` documented as executed, with exit codes. "X/Twitter cards" used consistently.
- `scripts/audit_build.py`: specific exceptions instead of a bare `except Exception`; unreadable files give a clear message; title and description length ranges are named constants. Behavior otherwise unchanged.

### Added
- `reference/crawler-controls.md`: neutral guide to Google AI feature controls, Google-Extended and vendor crawler tokens, each cited to the vendor's own page.
- `## Contents` lists in the reference files over 100 lines.

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
