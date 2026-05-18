# agentic-seo

A Claude Code skill for auditing, implementing, and optimizing SEO on web pages — built for Angular SSR/SSG but the principles apply to any framework.

Covers Google's AI optimization guide, structured data (JSON-LD), meta tags, Open Graph, Twitter Cards, canonical URLs, llms.txt, sitemap management, and AI-discoverability.

## Install

```bash
# Add the skill to your Claude Code project
claude skill add --from 23blocks-OS/agentic-seo
```

Or manually copy the `SKILL.md` and `reference/` folder into your `.claude/skills/agentic-seo/` directory.

## Usage

```
/agentic-seo audit /blocks/auth        # 33-check SEO audit with pass/fail table
/agentic-seo implement /new-page       # Full SEO implementation from scratch
/agentic-seo structured-data /page     # Add/update JSON-LD schemas
/agentic-seo llms-txt                  # Maintain llms.txt files
/agentic-seo checklist /page           # Quick 15-point verification
```

## Sub-Commands

| Command | What It Does |
|---------|-------------|
| `audit [path]` | Runs 33 checks (meta tags, OG, Twitter, JSON-LD, sitemap, HTML) and produces a pass/fail report |
| `implement [path]` | Step-by-step SEO implementation for a new page — imports, meta tags, structured data, sitemap |
| `structured-data [path]` | JSON-LD schema guide with templates for 7 schema types (SoftwareApplication, FAQPage, HowTo, etc.) |
| `llms-txt` | Maintain llms.txt and llms-full.txt for AI agent discoverability |
| `checklist [path]` | Quick 15-point verification against prerendered HTML with bash commands |

## What Gets Checked

The audit covers 33 items across 6 categories:

- **Meta Tags** (6): title, description, keywords, robots, author, canonical URL
- **Open Graph** (9): type, site_name, title, description, url, image, width, height, locale
- **Twitter Cards** (6): card, site, creator, title, description, image
- **Structured Data** (5): JSON-LD present, schema count, SSR-safe, cleanup, appropriate types
- **Infrastructure** (3): sitemap entry, prerender route, social image
- **HTML Template** (4): h1 tag, heading hierarchy, image alt text, internal links

## Key Principles

- **SSR-safe structured data**: JSON-LD must render during server-side rendering, never wrapped in browser-only guards
- **Minimum 2 schemas per page**: At least one content schema + BreadcrumbList
- **Unique meta descriptions**: Every page needs its own compelling description with a CTA
- **llms.txt is for non-Google agents**: Google uses standard crawling; llms.txt helps Claude, GPT, and other AI agents

## Structured Data Templates

The skill includes ready-to-use JSON-LD templates for:

- `SoftwareApplication` — product/feature pages
- `Organization` — company info
- `BreadcrumbList` — navigation context
- `FAQPage` — pages with FAQ content (rich snippets)
- `HowTo` — setup/installation guides
- `WebPage` — generic pages
- `Article` — blog posts

## Framework

Built for Angular SSR/SSG but the SEO principles, structured data patterns, and checklist methodology work with any web framework.

## License

MIT

---

Built by [23blocks](https://23blocks.com)
