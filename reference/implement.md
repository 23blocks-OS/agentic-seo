Implement full SEO on an Angular page component from scratch. Follow this step-by-step process to bring a page from zero to fully SEO-optimized.

## Inputs

- **Target**: Angular component path (e.g., `apps/web/src/app/pages/about/about.component.ts`)
- **Page title**: The primary keyword/topic of the page
- **Route path**: The URL path (e.g., `/about`)

## Step 1 -- Add Required Imports

Ensure the component has these imports:

```typescript
import { Component, OnInit, OnDestroy, Inject, PLATFORM_ID } from '@angular/core';
import { CommonModule, isPlatformBrowser, DOCUMENT } from '@angular/common';
import { Title, Meta } from '@angular/platform-browser';
```

If the component uses routing links, also include:
```typescript
import { RouterModule } from '@angular/router';
```

## Step 2 -- Add Constructor Dependencies

Add to the component's constructor:

```typescript
private jsonLdScript: HTMLScriptElement | null = null;

constructor(
  private titleService: Title,
  private metaService: Meta,
  @Inject(DOCUMENT) private document: Document,
  @Inject(PLATFORM_ID) private platformId: Object
) {}
```

Ensure the component implements `OnInit, OnDestroy`:
```typescript
export class MyComponent implements OnInit, OnDestroy {
```

## Step 3 -- Add Lifecycle Hooks

```typescript
ngOnInit() {
  this.setMetaTags();
  this.addStructuredData();
}

ngOnDestroy() {
  this.removeStructuredData();
}
```

## Step 4 -- Implement setMetaTags()

Create the method with ALL required tags. Use the page content to write compelling, unique copy.

```typescript
private setMetaTags() {
  // Title: 50-60 chars, primary keyword first, end with "| 23blocks"
  this.titleService.setTitle('Primary Keyword - Descriptor | 23blocks');

  // Description: 150-160 chars, include CTA, unique to this page
  this.metaService.updateTag({
    name: 'description',
    content: 'Compelling description with primary keyword. Explain the value. Include a call-to-action.'
  });

  // Keywords: 5-15 relevant terms
  this.metaService.updateTag({
    name: 'keywords',
    content: 'keyword1, keyword2, keyword3, 23blocks, related-term'
  });

  this.metaService.updateTag({ name: 'robots', content: 'index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1' });
  this.metaService.updateTag({ name: 'author', content: '23blocks' });

  // Canonical URL -- MUST match the page's route
  this.updateCanonicalLink('https://23blocks.com/ROUTE_PATH');

  // Open Graph (8+ tags)
  this.metaService.updateTag({ property: 'og:type', content: 'website' });
  this.metaService.updateTag({ property: 'og:site_name', content: '23blocks' });
  this.metaService.updateTag({ property: 'og:title', content: 'Same as title tag' });
  this.metaService.updateTag({ property: 'og:description', content: 'Same as meta description' });
  this.metaService.updateTag({ property: 'og:url', content: 'https://23blocks.com/ROUTE_PATH' });
  this.metaService.updateTag({ property: 'og:image', content: 'https://23blocks.com/assets/social/PAGE_NAME-og.png' });
  this.metaService.updateTag({ property: 'og:image:width', content: '1200' });
  this.metaService.updateTag({ property: 'og:image:height', content: '630' });
  this.metaService.updateTag({ property: 'og:image:alt', content: 'Descriptive alt text for the social image' });
  this.metaService.updateTag({ property: 'og:locale', content: 'en_US' });

  // Twitter Card (6+ tags)
  this.metaService.updateTag({ name: 'twitter:card', content: 'summary_large_image' });
  this.metaService.updateTag({ name: 'twitter:site', content: '@23blocks_co' });
  this.metaService.updateTag({ name: 'twitter:creator', content: '@23blocks_co' });
  this.metaService.updateTag({ name: 'twitter:title', content: 'Same as title tag' });
  this.metaService.updateTag({ name: 'twitter:description', content: 'Same as meta description' });
  this.metaService.updateTag({ name: 'twitter:image', content: 'https://23blocks.com/assets/social/PAGE_NAME-og.png' });
  this.metaService.updateTag({ name: 'twitter:image:alt', content: 'Same alt text as OG image' });

  // Theme color
  this.metaService.updateTag({ name: 'theme-color', content: '#111827' });
}
```

## Step 5 -- Implement updateCanonicalLink()

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

## Step 6 -- Implement addStructuredData()

CRITICAL: This method must NOT be wrapped in `isPlatformBrowser`. It must execute during SSR/SSG so that the JSON-LD appears in the prerendered HTML.

Choose appropriate schema types based on the page content (see `reference/structured-data.md` for the full guide).

Minimum schemas per page:
1. One content-specific schema (SoftwareApplication, WebPage, FAQPage, etc.)
2. BreadcrumbList

```typescript
private addStructuredData() {
  const contentSchema = {
    '@context': 'https://schema.org',
    '@type': 'WebPage',  // or SoftwareApplication, FAQPage, etc.
    'name': 'Page Name',
    'description': 'Same as meta description',
    'url': 'https://23blocks.com/ROUTE_PATH',
    'datePublished': 'YYYY-MM-DD',
    'dateModified': 'YYYY-MM-DD',
    'author': {
      '@type': 'Organization',
      'name': '23blocks',
      'url': 'https://23blocks.com'
    }
  };

  const breadcrumbSchema = {
    '@context': 'https://schema.org',
    '@type': 'BreadcrumbList',
    'itemListElement': [
      {
        '@type': 'ListItem',
        'position': 1,
        'name': 'Home',
        'item': 'https://23blocks.com'
      },
      {
        '@type': 'ListItem',
        'position': 2,
        'name': 'Page Name',
        'item': 'https://23blocks.com/ROUTE_PATH'
      }
    ]
  };

  this.jsonLdScript = this.document.createElement('script');
  this.jsonLdScript.type = 'application/ld+json';
  this.jsonLdScript.text = JSON.stringify([contentSchema, breadcrumbSchema]);
  this.document.head.appendChild(this.jsonLdScript);
}
```

## Step 7 -- Implement removeStructuredData()

```typescript
private removeStructuredData() {
  if (this.jsonLdScript && this.jsonLdScript.parentNode) {
    this.jsonLdScript.parentNode.removeChild(this.jsonLdScript);
  }
}
```

## Step 8 -- Update Sitemap

Add entry to `apps/web/public/sitemap.xml`:

```xml
<url>
  <loc>https://23blocks.com/ROUTE_PATH</loc>
  <lastmod>YYYY-MM-DD</lastmod>
  <changefreq>weekly</changefreq>
  <priority>0.7</priority>
</url>
```

Priority guidelines:
- `1.0` -- Homepage only
- `0.9` -- Main product pages (blocks, start)
- `0.8` -- Major feature pages (auth block, developers)
- `0.7` -- Feature sub-pages, marketing pages
- `0.6` -- Use cases, company pages
- `0.5` -- Support, partners
- `0.3` -- Legal pages

## Step 9 -- Update Routes for Prerendering

Verify the route is listed in `apps/web/routes.txt` so Angular SSG prerenders it.

## Step 10 -- Verify HTML Template

Check the component's `.html` file for:
- Exactly one `<h1>` tag
- Logical heading hierarchy (h1 > h2 > h3, no skipped levels)
- All `<img>` tags have descriptive `alt` attributes
- Internal links to related 23blocks pages via `routerLink`

## Step 11 -- Social Sharing Image

Note: A 1200x630 PNG should exist at `apps/web/src/assets/social/PAGE_NAME-og.png`. If it doesn't exist, flag it as a TODO but don't block the implementation.

## Post-Implementation

After implementing, run:
```
/agentic-seo audit /ROUTE_PATH
```
to verify all 33 checks pass.

## Checklist Before Done

- [ ] All imports added
- [ ] Constructor dependencies injected
- [ ] `setMetaTags()` with all tags
- [ ] `updateCanonicalLink()` helper
- [ ] `addStructuredData()` with 2+ schemas, SSR-safe
- [ ] `removeStructuredData()` in ngOnDestroy
- [ ] Sitemap entry added
- [ ] Route in routes.txt
- [ ] HTML template has proper h1, heading hierarchy, alt text
- [ ] Social image noted (exists or flagged as TODO)
