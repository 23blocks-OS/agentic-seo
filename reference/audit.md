Run a systematic SEO audit on a target page and generate a comprehensive pass/fail report. Don't fix issues; document them for the `implement` or `structured-data` sub-commands to address.

## Inputs

- **Target**: Angular component path (e.g., `apps/web/src/app/blocks/auth/auth.component.ts`) or route path (e.g., `/blocks/auth`)
- If a route path is given, resolve it to the component file via the app routing configuration

## Audit Steps

### Step 1 -- Read the Component

Read the target component's `.ts` file. Identify:
- `setMetaTags()` or equivalent method
- `addStructuredData()` or equivalent method
- `updateCanonicalLink()` or equivalent method
- `ngOnDestroy()` cleanup
- Constructor dependencies (Title, Meta, DOCUMENT, PLATFORM_ID)

### Step 2 -- Check Meta Tags

Verify each tag exists and meets quality standards:

| # | Check | Requirement | Pass Criteria |
|---|-------|-------------|---------------|
| 1 | Title tag | `this.titleService.setTitle(...)` | 50-60 chars, includes primary keyword, ends with `\| 23blocks` |
| 2 | Meta description | `name: 'description'` | 150-160 chars, includes CTA, unique to page |
| 3 | Meta keywords | `name: 'keywords'` | Relevant terms, not stuffed (5-15 keywords) |
| 4 | Meta robots | `name: 'robots'` | `index, follow` at minimum |
| 5 | Meta author | `name: 'author'` | `23blocks` |
| 6 | Canonical URL | `updateCanonicalLink(...)` | Full URL `https://23blocks.com/...`, matches page route |

### Step 3 -- Check Open Graph Tags

All 8+ required:

| # | Check | Tag | Pass Criteria |
|---|-------|-----|---------------|
| 7 | OG type | `og:type` | `website` (or `article` for blog posts) |
| 8 | OG site_name | `og:site_name` | `23blocks` |
| 9 | OG title | `og:title` | Same as or similar to title tag |
| 10 | OG description | `og:description` | Same as or similar to meta description |
| 11 | OG url | `og:url` | Matches canonical URL |
| 12 | OG image | `og:image` | Full URL to 1200x630 PNG at `https://23blocks.com/assets/social/` |
| 13 | OG image:width | `og:image:width` | `1200` |
| 14 | OG image:height | `og:image:height` | `630` |
| 15 | OG locale | `og:locale` | `en_US` |

### Step 4 -- Check Twitter Card Tags

All 6+ required:

| # | Check | Tag | Pass Criteria |
|---|-------|-----|---------------|
| 16 | Twitter card | `twitter:card` | `summary_large_image` |
| 17 | Twitter site | `twitter:site` | `@23blocks_co` |
| 18 | Twitter creator | `twitter:creator` | `@23blocks_co` |
| 19 | Twitter title | `twitter:title` | Same as or similar to title tag |
| 20 | Twitter description | `twitter:description` | Same as or similar to meta description |
| 21 | Twitter image | `twitter:image` | Same as OG image |

### Step 5 -- Check Structured Data (JSON-LD)

| # | Check | Requirement | Pass Criteria |
|---|-------|-------------|---------------|
| 22 | JSON-LD present | `addStructuredData()` method exists | Method creates `<script type="application/ld+json">` |
| 23 | Schema count | At least 2 schemas | Array or multiple scripts with distinct `@type` values |
| 24 | SSR-safe | NOT wrapped in `isPlatformBrowser` | `addStructuredData()` called directly in `ngOnInit()` without browser guard |
| 25 | Cleanup | `removeStructuredData()` in `ngOnDestroy()` | Removes script element from DOM |
| 26 | Schema types | Appropriate types used | At minimum: one content schema + BreadcrumbList |

### Step 6 -- Check Infrastructure

| # | Check | Requirement | Pass Criteria |
|---|-------|-------------|---------------|
| 27 | Sitemap entry | Entry in `apps/web/public/sitemap.xml` | `<url>` with correct `<loc>`, appropriate `<priority>` |
| 28 | Routes.txt | Entry in `apps/web/routes.txt` | Route path listed for prerendering |
| 29 | Social image | File exists at `apps/web/src/assets/social/` | PNG file referenced by OG/Twitter image tags |

### Step 7 -- Check HTML Template

Read the component's `.html` template file and verify:

| # | Check | Requirement | Pass Criteria |
|---|-------|-------------|---------------|
| 30 | H1 tag | Exactly one `<h1>` per page | One `h1` in template |
| 31 | Heading hierarchy | Logical h1 > h2 > h3 progression | No skipped levels (e.g., h1 then h3) |
| 32 | Image alt text | All `<img>` tags have descriptive `alt` | No empty or missing alt attributes |
| 33 | Internal links | Links to related 23blocks pages | At least 1 `routerLink` to another page |

## Generate Report

### SEO Audit Report: [Page Name]

**Component**: `path/to/component.ts`
**Route**: `/route-path`
**Date**: YYYY-MM-DD

| # | Check | Status | Detail |
|---|-------|--------|--------|
| 1 | Title tag | PASS/FAIL | [actual value or what's missing] |
| 2 | Meta description | PASS/FAIL | [char count, content preview] |
| ... | ... | ... | ... |
| 33 | Internal links | PASS/FAIL | [count of internal links found] |

### Summary

- **Total checks**: 33
- **Passed**: X
- **Failed**: Y
- **Score**: X/33 (XX%)

### Critical Issues (Fix First)

List any FAIL items that affect crawlability or indexing:
- Missing canonical URL
- No JSON-LD structured data
- Not in sitemap
- Not in routes.txt for prerendering

### Recommendations

Ordered list of what to fix, with the appropriate sub-command:
1. `agentic-seo implement /path` -- if multiple meta tags are missing
2. `agentic-seo structured-data /path` -- if JSON-LD is missing or incomplete
3. `agentic-seo checklist /path` -- after fixes, to verify against built HTML

**NEVER**:
- Report a PASS when the check is partially met
- Skip checking the HTML template
- Ignore the sitemap and routes.txt checks
- Assume structured data is SSR-safe without verifying the code path
