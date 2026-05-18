---
name: agentic-seo
description: Audit, implement, and optimize SEO for 23blocks web pages following Google's AI optimization guide. Covers meta tags, Open Graph, Twitter Cards, JSON-LD structured data, canonical URLs, llms.txt, sitemap, and AI-discoverability.
version: 1.0.0
user-invocable: true
argument-hint: "[audit|implement|structured-data|llms-txt|checklist] [target]"
metadata:
  org: 23blocks
  applies-to: 23blocks web application (Angular SSR/SSG)
---

# Agentic SEO -- 23blocks Standard

Repeatable workflow for auditing and implementing SEO on any page in the 23blocks Angular web application. Covers Google's AI optimization guide, structured data, and the 23blocks SEO standards defined in CLAUDE.md.

## When to use this skill

Trigger this skill when:
- A **new page** is created and needs SEO implementation
- An **SEO audit** is requested on an existing page
- **Structured data** (JSON-LD) needs to be added or updated
- **llms.txt** or **llms-full.txt** needs maintenance
- A quick **checklist** verification is needed before deploying
- A page is missing meta tags, OG tags, canonical URL, or sitemap entry

## Google AI Optimization Principles

Key rules from Google's official guide for AI-era SEO:

### Crawlability
- No `robots.txt` blocks on important content
- Proper canonical URLs on every page
- Pages must return correct HTTP status codes (200, 301, 404)
- Prerendered pages must contain full content in the HTML source

### Content Quality
- Unique, descriptive `<title>` and `<meta description>` per page
- Semantic HTML: proper `h1`-`h6` hierarchy, one `h1` per page
- Internal linking between related pages
- Content must be useful to humans first, machines second

### Structured Data
- JSON-LD in `<head>`, rendered server-side during SSR/SSG
- At least 2 schema types per page (e.g., SoftwareApplication + BreadcrumbList)
- CRITICAL: JSON-LD must NOT be wrapped in `isPlatformBrowser` guard -- it must render during SSR

### Technical
- Fast load times (prerendered static HTML via Angular SSG)
- Mobile-friendly responsive design
- Valid HTML, no broken links

### What NOT to Do
- Don't create special hidden content for Google AI (that's cloaking)
- Don't stuff keywords unnaturally
- Don't duplicate content across pages
- `llms.txt` is for non-Google agents; Google uses standard crawling

## The 23blocks SEO Standard

Every Angular component that represents a routable page must implement these patterns.

### Required Imports

```typescript
import { Component, OnInit, OnDestroy, Inject, PLATFORM_ID } from '@angular/core';
import { CommonModule, isPlatformBrowser, DOCUMENT } from '@angular/common';
import { Title, Meta } from '@angular/platform-browser';
```

### Required Constructor Dependencies

```typescript
constructor(
  private titleService: Title,
  private metaService: Meta,
  @Inject(DOCUMENT) private document: Document,
  @Inject(PLATFORM_ID) private platformId: Object
) {}
```

### Lifecycle Hooks

```typescript
private jsonLdScript: HTMLScriptElement | null = null;

ngOnInit() {
  this.setMetaTags();
  this.addStructuredData();
}

ngOnDestroy() {
  this.removeStructuredData();
}
```

### setMetaTags() Pattern

Must include all of:
1. Title (50-60 chars, includes primary keyword)
2. Meta description (150-160 chars, includes CTA)
3. Keywords, robots, author
4. Canonical URL via `updateCanonicalLink()`
5. 8+ Open Graph tags
6. 6+ Twitter Card tags

### updateCanonicalLink() Helper

```typescript
private updateCanonicalLink(url: string) {
  const existingLink = this.document.querySelector('link[rel="canonical"]');
  if (existingLink) {
    existingLink.setAttribute('href', url);
  } else {
    const link = this.document.createElement('link');
    link.setAttribute('rel', 'canonical');
    link.setAttribute('href', url);
    this.document.head.appendChild(link);
  }
}
```

### addStructuredData() -- NO isPlatformBrowser Guard

```typescript
private addStructuredData() {
  const schemas = [ /* ... schema objects ... */ ];
  this.jsonLdScript = this.document.createElement('script');
  this.jsonLdScript.type = 'application/ld+json';
  this.jsonLdScript.text = JSON.stringify(schemas);
  this.document.head.appendChild(this.jsonLdScript);
}
```

### removeStructuredData()

```typescript
private removeStructuredData() {
  if (this.jsonLdScript && this.jsonLdScript.parentNode) {
    this.jsonLdScript.parentNode.removeChild(this.jsonLdScript);
  }
}
```

## Sub-Commands

| Command | Description | Reference |
|---------|-------------|-----------|
| `audit [path]` | Full SEO audit of a page -- produces pass/fail table | `reference/audit.md` |
| `implement [path]` | Implement SEO on a new page from scratch | `reference/implement.md` |
| `structured-data [path]` | Add or update JSON-LD structured data | `reference/structured-data.md` |
| `llms-txt` | Maintain llms.txt and llms-full.txt files | `reference/llms-txt.md` |
| `checklist [path]` | Quick 15-item verification against prerendered HTML | `reference/checklist.md` |

When no sub-command is given, default to `audit` on the target path.

## Reference Implementation

The canonical example of a fully SEO-optimized page is:
`apps/web/src/app/agents/agents-landing.component.ts`

This component demonstrates all patterns: meta tags, OG, Twitter Cards, canonical URL, 4 JSON-LD schemas (SoftwareApplication, Organization, BreadcrumbList, FAQPage), and SSR-safe structured data injection.

## Anti-Patterns

- **isPlatformBrowser on JSON-LD**: Never guard structured data with browser checks -- it must render during SSR/SSG
- **Missing ngOnDestroy cleanup**: Always remove JSON-LD scripts to prevent duplication on SPA navigation
- **Hardcoded dates**: Use the current date for `dateModified` in schemas
- **Generic descriptions**: Every page needs a unique, specific meta description
- **Missing sitemap entry**: Every new page must be added to `apps/web/public/sitemap.xml`
- **Missing routes.txt entry**: Every new page must be in `apps/web/routes.txt` for prerendering
