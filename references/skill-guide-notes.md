# Skill Guide Notes

Extracted from Anthropic's "The Complete Guide to Building Skills for Claude" (Feb 2026).

## File Structure

```
your-skill-name/
├── SKILL.md              # Required - main skill file
├── scripts/              # Optional - executable code
├── references/           # Optional - documentation loaded as needed
├── examples/             # Optional
└── assets/               # Optional - templates, fonts, icons
```

## YAML Frontmatter (required)

```yaml
---
name: your-skill-name
description: What it does. Use when user asks to [specific phrases]. Key capabilities include X, Y, Z.
---
```

- `name`: kebab-case, must match folder name
- `description`: under 1024 chars, no XML tags, include trigger phrases users would say
- Optional: `license`, `compatibility`, `metadata` (author, version, mcp-server)

## Description Formula

```
[What it does] + [When to use it] + [Key capabilities]
```

Good: "Analyzes Figma design files and generates developer handoff documentation. Use when user uploads .fig files, asks for 'design specs', 'component documentation', or 'design-to-code handoff'."

Bad: "Helps with projects." (too vague, no triggers)

## Progressive Disclosure (three levels)

1. **Frontmatter** (always in system prompt): just enough to know when to load
2. **SKILL.md body** (loaded when relevant): full instructions
3. **references/** (loaded on demand): detailed docs, examples

## Best Practices

- Be specific and actionable (not "validate the data" but "run `python scripts/validate.py --input {filename}`")
- Include error handling sections
- Reference bundled resources clearly
- Keep SKILL.md under 5,000 words
- Put critical instructions at the top
- Use bullet points and numbered lists
- No README.md inside skill folders

## Testing

Three areas:
1. **Triggering**: Does it load at the right times? (positive + negative test cases)
2. **Functional**: Does it produce correct outputs?
3. **Performance**: Does it improve results vs. no skill?

## Distribution

- Host on GitHub with clear README (repo-level, not inside skill folders)
- Installation: download folder -> zip -> upload to Claude.ai Settings > Skills, or place in Claude Code skills directory
- Skills are an open standard (cross-platform, not Claude-specific)
