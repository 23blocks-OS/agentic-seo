# Contributing to agentic-seo

Thanks for helping. This is a Claude Code skill: mostly Markdown (`SKILL.md` and `reference/`), plus one standard-library Python script, `scripts/audit_build.py`.

## Proposing a change

1. Open an issue first for anything larger than a typo.
2. Fork, branch, and keep the change small and focused.
3. Update `CHANGELOG.md`. If the behavior of the skill changes, bump `metadata.version` in `SKILL.md`.
4. Open a pull request that says what changed and why.

## Checks to run

```bash
python3 scripts/audit_build.py --self-test
```

Run it after any change to the script. It builds a clean fixture and plants defects, and fails if a defect is missed.

## Writing guidelines

- Claims about search engines must be accurate and dated, with a link to the source. Do not claim that structured data, llms.txt or special markup improves visibility in Google AI features; Google's guide says it does not.
- Add structured data advice only where it matches visible page content.
