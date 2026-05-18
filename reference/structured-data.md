Guide for adding and updating JSON-LD structured data on 23blocks pages. Each schema type includes a complete template with 23blocks-specific values.

## Critical Rules

1. **SSR-safe**: JSON-LD must NOT be wrapped in `isPlatformBrowser`. It must render during Angular SSG/SSR so Google sees it in the HTML source.
2. **Minimum 2 schemas**: Every page needs at least one content schema + BreadcrumbList.
3. **Single script tag**: Combine all schemas into one `<script type="application/ld+json">` using a JSON array.
4. **Cleanup on destroy**: Always remove the script in `ngOnDestroy()` to prevent duplication during SPA navigation.

## Schema Types and When to Use Them

### SoftwareApplication

**Use on**: Product pages, block pages, feature pages

```typescript
const schema = {
  '@context': 'https://schema.org',
  '@type': 'SoftwareApplication',
  'name': '23blocks [Block Name]',
  'applicationCategory': 'DeveloperApplication',
  'operatingSystem': 'Web',
  'description': 'Description of the block/feature',
  'url': 'https://23blocks.com/blocks/BLOCK_NAME',
  'datePublished': 'YYYY-MM-DD',
  'dateModified': 'YYYY-MM-DD',
  'author': {
    '@type': 'Organization',
    'name': '23blocks',
    'url': 'https://23blocks.com'
  },
  'offers': {
    '@type': 'Offer',
    'price': '0',
    'priceCurrency': 'USD',
    'description': 'Free tier available'
  },
  'featureList': [
    'Feature 1',
    'Feature 2',
    'Feature 3'
  ]
};
```

### Organization

**Use on**: All pages (can be combined with any other schema). Include at minimum on homepage, about, and contact pages.

```typescript
const schema = {
  '@context': 'https://schema.org',
  '@type': 'Organization',
  'name': '23blocks',
  'url': 'https://23blocks.com',
  'logo': 'https://23blocks.com/assets/logo.png',
  'sameAs': [
    'https://twitter.com/23blocks_co',
    'https://github.com/23blocks-OS',
    'https://linkedin.com/company/23blocks'
  ]
};
```

### BreadcrumbList

**Use on**: Every page (required as one of the minimum 2 schemas).

```typescript
const schema = {
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
      'name': 'Section Name',
      'item': 'https://23blocks.com/section'
    },
    {
      '@type': 'ListItem',
      'position': 3,
      'name': 'Page Name',
      'item': 'https://23blocks.com/section/page'
    }
  ]
};
```

### FAQPage

**Use on**: Any page with FAQ content. Produces rich snippets in Google search results (expandable Q&A).

```typescript
const schema = {
  '@context': 'https://schema.org',
  '@type': 'FAQPage',
  'mainEntity': [
    {
      '@type': 'Question',
      'name': 'Question text here?',
      'acceptedAnswer': {
        '@type': 'Answer',
        'text': 'Plain text answer (strip HTML tags from any rich content)'
      }
    }
  ]
};
```

**Dynamic FAQ from component data**:
```typescript
const faqSchema = {
  '@context': 'https://schema.org',
  '@type': 'FAQPage',
  'mainEntity': this.faqs.map(faq => ({
    '@type': 'Question',
    'name': faq.question,
    'acceptedAnswer': {
      '@type': 'Answer',
      'text': faq.answer.replace(/<[^>]*>/g, '')  // Strip HTML
    }
  }))
};
```

### HowTo

**Use on**: Installation guides, setup pages, getting-started pages.

```typescript
const schema = {
  '@context': 'https://schema.org',
  '@type': 'HowTo',
  'name': 'How to Set Up [Feature] with 23blocks',
  'description': 'Step-by-step guide to...',
  'step': [
    {
      '@type': 'HowToStep',
      'position': 1,
      'name': 'Create an Account',
      'text': 'Sign up at 23blocks.com and create your first application.',
      'url': 'https://23blocks.com/start'
    },
    {
      '@type': 'HowToStep',
      'position': 2,
      'name': 'Install the SDK',
      'text': 'Run npm install @23blocks/gateway to add the SDK to your project.',
      'url': 'https://23blocks.com/developers/docs/getting-started'
    }
  ],
  'totalTime': 'PT5M'
};
```

### WebPage

**Use on**: Generic pages that don't fit other types (about, contact, legal).

```typescript
const schema = {
  '@context': 'https://schema.org',
  '@type': 'WebPage',
  'name': 'Page Title',
  'description': 'Page description',
  'url': 'https://23blocks.com/page-path',
  'datePublished': 'YYYY-MM-DD',
  'dateModified': 'YYYY-MM-DD',
  'publisher': {
    '@type': 'Organization',
    'name': '23blocks',
    'url': 'https://23blocks.com'
  }
};
```

### Article

**Use on**: Blog posts, case studies, news articles.

```typescript
const schema = {
  '@context': 'https://schema.org',
  '@type': 'Article',
  'headline': 'Article Title (max 110 chars)',
  'description': 'Article description',
  'url': 'https://23blocks.com/blog/article-slug',
  'datePublished': 'YYYY-MM-DD',
  'dateModified': 'YYYY-MM-DD',
  'author': {
    '@type': 'Organization',
    'name': '23blocks',
    'url': 'https://23blocks.com'
  },
  'publisher': {
    '@type': 'Organization',
    'name': '23blocks',
    'url': 'https://23blocks.com',
    'logo': {
      '@type': 'ImageObject',
      'url': 'https://23blocks.com/assets/logo.png'
    }
  },
  'image': 'https://23blocks.com/assets/social/article-og.png'
};
```

## Combining Schemas

Always combine all schemas for a page into a single JSON array:

```typescript
private addStructuredData() {
  const schemas = [contentSchema, breadcrumbSchema, faqSchema]; // etc.

  this.jsonLdScript = this.document.createElement('script');
  this.jsonLdScript.type = 'application/ld+json';
  this.jsonLdScript.text = JSON.stringify(schemas);
  this.document.head.appendChild(this.jsonLdScript);
}
```

## Choosing the Right Schema Combination

| Page Type | Primary Schema | Additional Schemas |
|-----------|---------------|-------------------|
| Block/product page | SoftwareApplication | BreadcrumbList, Organization |
| Block page with FAQ | SoftwareApplication | BreadcrumbList, FAQPage |
| Getting started | HowTo | BreadcrumbList, Organization |
| About/company | Organization | BreadcrumbList, WebPage |
| Legal pages | WebPage | BreadcrumbList |
| Blog post | Article | BreadcrumbList, Organization |
| Agent/feature page | SoftwareApplication | BreadcrumbList, FAQPage, Organization |
| Homepage | Organization | WebPage |

## Validation

After implementing, verify structured data appears in prerendered HTML:

```bash
grep 'application/ld+json' dist/apps/web/browser/ROUTE_PATH/index.html
```

The JSON-LD should be present in the `<head>` of the prerendered file. If it's missing, the `addStructuredData()` method is likely guarded by `isPlatformBrowser` or not being called during SSR.
