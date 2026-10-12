# agentic-seo: Claude Code skill for SEO audits and implementation

[![Claude Code skill](https://img.shields.io/badge/Claude_Code-skill-D97757)](https://docs.claude.com/en/docs/claude-code/skills)
[![License: MIT](https://img.shields.io/github/license/23blocks-OS/agentic-seo)](LICENSE)

**agentic-seo is an agent skill (`SKILL.md`) that lets Claude Code audit and implement SEO for a website:** meta tags, canonical URLs, Open Graph, X/Twitter cards, JSON-LD structured data, sitemap entries and crawler controls.

It works with any website and any framework that renders HTML on the server or at build time (Angular, Next.js, Nuxt, Astro, SvelteKit and others). Code examples use Angular syntax; the checks are framework-agnostic.

## What it does

- **Audits one page** against a 33-point checklist and returns a pass/fail table (`audit`).
- **Audits a whole site** from its built HTML with a Python script, optionally diffed against the live build to catch regressions on pages that already rank (`audit-build`).
- **Implements SEO on a new page**: title, description, canonical, Open Graph, X/Twitter cards, JSON-LD, sitemap and prerender route (`implement`).
- **Adds JSON-LD** only where it matches visible content (`structured-data`).
- **Maintains `llms.txt`** if the site owner wants one, and **explains crawler controls** for Google and AI vendors without taking sides (`llms-txt`, `crawler-controls`).
- **Verifies built HTML** with a 15-point checklist (`checklist`).

## Install

### Option 1: through the AI Maestro plugin

The skill is part of the `ai-maestro` plugin from [ai-maestro-plugins](https://github.com/23blocks-OS/ai-maestro-plugins):

```bash
claude plugin marketplace add 23blocks-OS/ai-maestro-plugins
claude plugin install ai-maestro@ai-maestro-plugins
```

Claude Code namespaces plugin skills, so the command may appear as `/ai-maestro:agentic-seo`.

### Option 2: standalone

```bash
git clone https://github.com/23blocks-OS/agentic-seo.git ~/.claude/skills/agentic-seo
```

The whole-site audit needs Python 3 (standard library only). If you only want the page-level workflows, copy `SKILL.md` and the `reference/` folder into `~/.claude/skills/agentic-seo/`; add `scripts/` too for `audit-build`.

## Usage

```bash
/agentic-seo audit /blocks/auth          # audit one page
/agentic-seo implement /new-feature      # build SEO for a new page
/agentic-seo structured-data /about      # add or update JSON-LD
/agentic-seo checklist /blocks/auth      # quick check of the built HTML
/agentic-seo audit-build dist/web        # audit every built page
```

The build audit can also run directly:

```bash
python3 scripts/audit_build.py BUILD_DIR --domain https://acme.com [--baseline BASE_DIR] [--csv out.csv]
python3 scripts/audit_build.py --self-test
```

Exit code `0` means no page failed a hard check; `1` means at least one did; `2` means bad arguments.

On first use the skill asks for your site configuration (name, domain, X/Twitter handle, social image path, framework, build output and sitemap path), or reads it from `CLAUDE.md` if your project already defines it.

## Sub-commands

| Command | What it does |
|---------|-------------|
| `/agentic-seo audit [path]` | 33-check SEO audit of one page, pass/fail table |
| `/agentic-seo implement [path]` | SEO implementation for a new page |
| `/agentic-seo structured-data [path]` | Add or update JSON-LD schemas |
| `/agentic-seo llms-txt` | Maintain `llms.txt`, only if the owner wants it |
| `/agentic-seo crawler-controls` | Neutral guide to Google AI feature controls and vendor crawler tokens |
| `/agentic-seo checklist [path]` | 15-point verification against built HTML |
| `/agentic-seo audit-build [dir]` | Whole-site audit of built HTML, with an optional diff against the live build |

## The 33-Point Audit

The `audit` command checks every item that matters for modern SEO:

### Meta Tags (6 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 1 | Title tag | 50-60 chars, includes primary keyword |
| 2 | Meta description | 150-160 chars, unique, includes CTA |
| 3 | Keywords | Advisory only: Google ignores it |
| 4 | Robots | `index, follow` directive |
| 5 | Author | Present |
| 6 | Canonical URL | Full URL matching the page route |

### Open Graph (9 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 7-9 | type, site_name, title | Correct values |
| 10-11 | description, url | Match meta/canonical |
| 12-15 | image, width, height, locale | 1200x630 PNG, en_US |

### X/Twitter cards (6 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 16-21 | card, site, creator, title, description, image | `summary_large_image`, matching values |

### Structured Data (5 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 22 | JSON-LD present | `<script type="application/ld+json">` |
| 23 | Schema fit | Each schema matches visible content; no minimum count |
| 24 | SSR-safe | Not wrapped in browser-only guard |
| 25 | Cleanup | Removed on component destroy |
| 26 | Appropriate types | Types fit the page (for example Article, BreadcrumbList); FAQPage and HowTo not needed |

### Infrastructure (3 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 27 | Sitemap entry | In sitemap.xml with a correct `<loc>` (Google ignores `<priority>` and `<changefreq>`) |
| 28 | Prerender route | Configured for static generation |
| 29 | Social image | 1200x630 PNG exists |

### HTML Template (4 checks)
| # | Check | Pass Criteria |
|---|-------|---------------|
| 30 | H1 tag | Exactly one per page |
| 31 | Heading hierarchy | h1 > h2 > h3, no skipped levels |
| 32 | Image alt text | All images have descriptive alt |
| 33 | Internal links | At least 1 link to related page |

## JSON-LD templates

Templates for six schema types, to use only where the type matches visible page content:

| Schema Type | Use Case |
|-------------|----------|
| `SoftwareApplication` | Product and feature pages |
| `Organization` | Company info |
| `BreadcrumbList` | Navigation context on deep pages |
| `Article` | Blog posts, case studies |
| `WebPage` | Generic pages (about, contact, legal); valid markup, no rich result |
| `FAQPage` | Optional. Google stopped showing FAQ rich results on 2026-05-07 |

See [`reference/structured-data.md`](reference/structured-data.md). Run Google's Rich Results Test and the Schema Markup Validator before calling a page done.

## What Google says about AI search

As of 2026-10-10, Google's guide [Optimizing your website for generative AI features on Google Search](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) (last updated 2026-07-10) says:

- You do not need new machine-readable files, AI text files, markup or Markdown to appear in Google Search.
- Structured data is not required for generative AI search, and there is no special schema.org markup to add.
- Google Search does not use `llms.txt`; having one neither harms nor helps visibility or rankings there.
- Content does not need to be broken into tiny pieces.

So this skill does not promise that structured data, `llms.txt` or special markup improves visibility in Google AI features. Standard SEO applies: crawlable, indexed pages with useful content. Structured data is still worth adding for rich-result eligibility where Google supports the type. Re-check the guide before relying on this; it changes.

## Key principles

1. **SSR-safe structured data.** JSON-LD must be in the server-rendered or prerendered HTML, not behind a browser-only guard.
2. **Schema that matches the page.** Never add a schema type to reach a count.
3. **Unique meta descriptions and canonical URLs** on every page.
4. **Sitemap and prerender route** for every page that should be found. Set `lastmod` only when it is accurate; Google ignores `priority` and `changefreq`.
5. **Measure performance** with Core Web Vitals (LCP, INP, CLS).

## File structure

```
agentic-seo/
├── SKILL.md                  # skill definition
├── README.md
├── CHANGELOG.md
├── LICENSE                   # MIT
├── scripts/audit_build.py    # whole-site audit of built HTML
└── reference/
    ├── audit.md  audit-build.md  checklist.md  implement.md
    ├── structured-data.md  llms-txt.md  crawler-controls.md
```

## Part of the AI Maestro ecosystem

agentic-seo ships in the `ai-maestro` plugin for [AI Maestro](https://ai-maestro.23blocks.com), a dashboard and orchestrator for fleets of AI coding agents ([source on GitHub](https://github.com/23blocks-OS/ai-maestro)). It also works on its own in any Claude Code setup.

## Related skills

- [agentic-sql](https://github.com/23blocks-OS/agentic-sql): safe, read-only SQL against production databases, with a saved query library.
- [agentic-support](https://github.com/23blocks-OS/agentic-support): numbered support cases and runbooks.

## Changelog

See [CHANGELOG.md](CHANGELOG.md). Contributions are welcome; see [CONTRIBUTING.md](.github/CONTRIBUTING.md).

## References

- [Google: Optimizing for generative AI features](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)
- [Google: AI features and your website](https://developers.google.com/search/docs/appearance/ai-features)
- [Google: Succeeding in AI search](https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search)
- [schema.org](https://schema.org)
- [llmstxt.org](https://llmstxt.org), the `llms.txt` specification
- [Open Graph Protocol](https://ogp.me)

## License

MIT. See [LICENSE](LICENSE).

---

Built by [23blocks](https://23blocks.com).
