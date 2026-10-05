# agentic-seo

A Claude Code skill that turns SEO from a manual checklist into a repeatable, invocable workflow. One command audits 33 items. Another implements everything from scratch. Five sub-commands cover the full lifecycle from audit to deploy.

Works with **any website and any framework**. On first use, the skill asks for your site configuration (domain, site name, Twitter handle, etc.) and uses those values throughout all sub-commands.

> **Based on:** Google's official [Optimizing your website for generative AI features on Google Search](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) guide (updated May 2026), the [llmstxt.org](https://llmstxt.org) specification, and [schema.org](https://schema.org) structured data standards.

## Why This Exists

Every page on a modern web app needs the same SEO work: title tags, meta descriptions, Open Graph, Twitter Cards, JSON-LD structured data, canonical URLs, sitemap entries, prerender routes. Doing it manually means copying patterns from docs, missing tags, and inconsistent quality across pages.

This skill encodes the entire process into 5 sub-commands that Claude Code can execute repeatably.

### First-Time Setup

On first use, the skill gathers your site configuration:

| Variable | Example |
|----------|---------|
| `{SITE_NAME}` | Acme Corp |
| `{DOMAIN}` | https://acme.com |
| `{TWITTER_HANDLE}` | @acme |
| `{SOCIAL_IMAGE_PATH}` | /assets/social/ |
| `{BUILD_OUTPUT}` | dist/apps/web/browser |
| `{SITEMAP_PATH}` | public/sitemap.xml |

If your project has a `CLAUDE.md` with SEO patterns already defined, the skill extracts these values automatically.

## Install

```bash
# Copy into your Claude Code skills directory
git clone https://github.com/23blocks-OS/agentic-seo.git ~/.claude/skills/agentic-seo
```

Or copy `SKILL.md` and the `reference/` folder into `.claude/skills/agentic-seo/`.

## Sub-Commands

| Command | What It Does |
|---------|-------------|
| `/agentic-seo audit [path]` | 33-check SEO audit with pass/fail table |
| `/agentic-seo implement [path]` | Full SEO implementation from scratch |
| `/agentic-seo structured-data [path]` | Add/update JSON-LD schemas (7 types) |
| `/agentic-seo llms-txt` | Maintain llms.txt and llms-full.txt |
| `/agentic-seo checklist [path]` | 15-point verification against built HTML |
| `/agentic-seo audit-build [dir]` | Whole-site audit of built HTML, with an optional diff against the live build |

```bash
/agentic-seo audit /blocks/auth          # Full audit
/agentic-seo implement /new-feature      # Build SEO from zero
/agentic-seo structured-data /about      # Add JSON-LD schemas
/agentic-seo llms-txt                    # Update AI agent discovery files
/agentic-seo checklist /blocks/auth      # Quick verify after build
```

## The 33-Point Audit

The `audit` command checks every item that matters for modern SEO:

### Meta Tags (6 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 1 | Title tag | 50-60 chars, includes primary keyword |
| 2 | Meta description | 150-160 chars, unique, includes CTA |
| 3 | Keywords | 5-15 relevant terms |
| 4 | Robots | `index, follow` directive |
| 5 | Author | Present |
| 6 | Canonical URL | Full URL matching the page route |

### Open Graph (9 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 7-9 | type, site_name, title | Correct values |
| 10-11 | description, url | Match meta/canonical |
| 12-15 | image, width, height, locale | 1200x630 PNG, en_US |

### Twitter Cards (6 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 16-21 | card, site, creator, title, description, image | `summary_large_image`, matching values |

### Structured Data (5 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 22 | JSON-LD present | `<script type="application/ld+json">` |
| 23 | Schema count | At least 2 types |
| 24 | SSR-safe | Not wrapped in browser-only guard |
| 25 | Cleanup | Removed on component destroy |
| 26 | Appropriate types | Content schema + BreadcrumbList |

### Infrastructure (3 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 27 | Sitemap entry | In sitemap.xml with correct priority |
| 28 | Prerender route | Configured for static generation |
| 29 | Social image | 1200x630 PNG exists |

### HTML Template (4 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 30 | H1 tag | Exactly one per page |
| 31 | Heading hierarchy | h1 > h2 > h3, no skipped levels |
| 32 | Image alt text | All images have descriptive alt |
| 33 | Internal links | At least 1 link to related page |

## JSON-LD Templates

Ready-to-use structured data templates for 7 schema types:

| Schema Type | Use Case |
|-------------|----------|
| `SoftwareApplication` | Product and feature pages |
| `Organization` | Company info (include on key pages) |
| `BreadcrumbList` | Navigation context (every page) |
| `FAQPage` | Pages with Q&A (enables rich snippets) |
| `HowTo` | Setup guides, installation pages |
| `WebPage` | Generic pages (about, contact, legal) |
| `Article` | Blog posts, case studies |

Each template includes complete JSON-LD with recommended field values. See [`reference/structured-data.md`](reference/structured-data.md).

## Google AI Optimization: What Matters

This skill is informed by Google's [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide). Key takeaways encoded in the skill:

**Do:**
- Create valuable, non-commodity content with a unique point of view
- Build clear technical structure (crawlable, indexed, proper headings)
- Use semantic HTML with logical heading hierarchy
- Make content eligible for Google Search snippets first
- Use structured data for rich results eligibility

**Don't:**
- Create special machine-readable files just for Google AI (Google crawls your pages directly)
- Break content into tiny chunks thinking AI prefers it
- Rewrite content in a specific way for generative AI search
- Pursue inauthentic mentions across the web

**On structured data**, Google clarifies it isn't required for AI search features, but remains valuable for rich results and overall SEO. This skill treats it as a best practice for complete SEO coverage.

**On llms.txt**, Google doesn't use it. The `llms-txt` sub-command exists for non-Google AI agents (Claude, GPT, etc.) that do respect the [llmstxt.org](https://llmstxt.org) standard.

## File Structure

```
agentic-seo/
├── SKILL.md                        # Main skill definition
├── README.md                       # This file
├── LICENSE                         # MIT License
└── reference/
    ├── audit.md                    # 33-check SEO audit workflow
    ├── implement.md                # Implement SEO on a new page
    ├── structured-data.md          # JSON-LD schema templates (7 types)
    ├── llms-txt.md                 # llms.txt maintenance guide
    └── checklist.md                # Quick 15-point build verification
```

## Key Principles

1. **SSR-safe structured data** -- JSON-LD must render during server-side rendering. Never wrap it in browser-only guards. If it's not in the prerendered HTML, search engines won't see it.

2. **Minimum 2 schemas per page** -- Every page gets at least one content schema (SoftwareApplication, WebPage, FAQPage, etc.) plus a BreadcrumbList for navigation context.

3. **Unique meta descriptions** -- Every page needs its own compelling, specific description with a call-to-action. Generic descriptions hurt click-through rates.

4. **Canonical URLs on every page** -- Prevents duplicate content issues, especially with SPA navigation and prerendered routes.

5. **Sitemap + prerender route** -- A page that isn't in the sitemap and isn't prerendered is invisible to search engines.

## Works With

Works with **any web framework** that supports SSR or SSG. Code examples use Angular syntax but the SEO principles, structured data patterns, audit methodology, and checklist verification are framework-agnostic:

- Angular (SSR/SSG)
- Next.js / React
- Nuxt / Vue
- Astro, SvelteKit, or any SSR/SSG framework

## References

- [Google: Optimizing for generative AI features](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) -- the official guide this skill is based on
- [Google: AI features and your website](https://developers.google.com/search/docs/appearance/ai-features) -- how AI features appear in search
- [Google: Succeeding in AI search](https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search) -- blog post on content strategy
- [schema.org](https://schema.org) -- structured data vocabulary
- [llmstxt.org](https://llmstxt.org) -- the llms.txt specification for AI agent discovery
- [Open Graph Protocol](https://ogp.me) -- social sharing metadata standard

## License

[MIT](LICENSE)

---

Built by [23blocks](https://23blocks.com) -- the backend-as-a-service with 18 blocks and 203 agent-ready skills.
