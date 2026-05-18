Guide for maintaining `llms.txt` and `llms-full.txt` files for AI agent discoverability. These files follow the llmstxt.org specification and help non-Google AI agents understand what 23blocks offers.

## File Locations

- **llms.txt**: `apps/web/public/llms.txt` -- concise summary (~65 lines)
- **llms-full.txt**: `apps/web/public/llms-full.txt` -- comprehensive reference (~600 lines)
- **Live URLs**: `https://23blocks.com/llms.txt` and `https://23blocks.com/llms-full.txt`

## llmstxt.org Format Specification

### Structure

```
# Product Name

> Short description (blockquote)

## Section Name

- [Link Title](https://url): Brief description of what this link covers

## Optional

- [Link Title](https://url): Non-essential links
```

### Rules

1. Start with `# Product Name` as H1
2. Follow with a `> blockquote` summary
3. Organize links into `## Sections`
4. Each link: `- [Title](url): description`
5. Put non-essential links under `## Optional`
6. Keep descriptions concise (one line per link)
7. Use full URLs (`https://23blocks.com/...`)

## When to Update

### llms.txt (concise version)

Update when:
- A new block is added to the platform
- A new major feature page is created (agents, skills, etc.)
- Key URLs change
- The product description changes
- New MCP plugins are released

Content to include:
- Product overview
- Block descriptions (one line each)
- Key feature pages
- Documentation link
- Getting started link
- API reference link

### llms-full.txt (comprehensive version)

Update when:
- New API endpoints are added
- New skills are added to a block
- Detailed feature descriptions change
- New integration guides are published
- Block capabilities expand

Content to include:
- Everything in llms.txt PLUS:
- Detailed block descriptions with feature lists
- All API endpoints per block
- All skills per block
- Integration instructions
- Authentication details
- Code examples (brief)

## Editing Workflow

1. **Read current file**: Check what's already there before editing
2. **Identify the change**: New block? New feature? Updated description?
3. **Find the right section**: Add new links to the appropriate `## Section`
4. **Maintain alphabetical order**: Within each section, keep links sorted
5. **Keep descriptions consistent**: Match the style of existing entries
6. **Update both files**: If the change affects llms.txt, it likely affects llms-full.txt too

## robots.txt Reference

Both files should be referenced in `apps/web/public/robots.txt`:

```
# AI Agent Discovery
# llms.txt: https://23blocks.com/llms.txt
# llms-full.txt: https://23blocks.com/llms-full.txt
```

Verify this reference exists when updating the llms files.

## Important Notes

- **llms.txt is NOT for Google**: Google uses standard crawling and structured data (JSON-LD). llms.txt is for non-Google AI agents (Claude, GPT, etc.)
- **Don't duplicate sitemap**: llms.txt is a curated summary, not a comprehensive URL list
- **Keep llms.txt concise**: It should be scannable by an AI in one pass (~65 lines max)
- **llms-full.txt can be detailed**: This is where you put comprehensive API and feature documentation
- **Test readability**: The content should make sense to an AI agent that has no prior context about 23blocks

## Validation

After updating, verify:

```bash
# Check file exists and has content
wc -l apps/web/public/llms.txt
wc -l apps/web/public/llms-full.txt

# Verify format starts correctly
head -5 apps/web/public/llms.txt

# Check robots.txt references
grep 'llms' apps/web/public/robots.txt
```
